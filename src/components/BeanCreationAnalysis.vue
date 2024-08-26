<template>
  <div class="bean-creation-analysis">

    <div style="margin-bottom: 20px;">
      <el-input v-model="search.duration"
                placeholder="输入创建耗时/(ms)"
                size="medium"
                style="width: 20%; margin: 12px;">
        <template slot="prepend">创建耗时/(ms)</template>
        <i slot="prefix" class="el-input__icon el-icon-search"></i>
      </el-input>
      <el-input v-model="search.beanName"
                placeholder="输入Bean名称"
                size="medium"
                style="width: 20%">
        <template slot="prepend">Bean名称</template>
        <i slot="prefix" class="el-input__icon el-icon-search"></i>
      </el-input>
    </div>

    <el-table
      v-loading="loading"
      :data="getTableData()"
      :default-sort="defaultSort"
      :indent="16"
      :row-key="(row) => row.uuid"
      :row-style="this.getRowStyle"
      element-loading-spinner="el-icon-loading"
      element-loading-text="拼命加载中"
      size="mini"
      style="width: 100%">

      <el-table-column
        label="Bean名称"
        min-width="100%">
        <template slot-scope="scope">
          <el-popover
            placement="right-start"
            trigger="hover"
            width="100%">

            <div v-if="scope.row">
              <el-card class="box-card">
                <div slot="header" class="clearfix">
                  <span>Spring Bean info</span>
                </div>
                <div v-for="(value, key) in scope.row.tags" class="text item">
                  <strong>{{ key[0].toUpperCase() + key.slice(1) }}</strong>: {{ value }}
                </div>
              </el-card>
            </div>

            <span slot="reference">
              <span class="bean-name"></span>
              {{ scope.row.name }}
            </span>

          </el-popover>
        </template>
      </el-table-column>

      <el-table-column
        label="创建耗时/(ms)"
        prop="duration"
        width="180px">
      </el-table-column>

      <el-table-column
        label="实际耗时/(ms)"
        prop="actualDuration"
        width="180px">
      </el-table-column>

      <el-table-column
        label="实际耗时明细/(ms)"
        width="auto">
        <template slot-scope="scope">
          <div v-if="scope.row.beanLifeCycles" class="block">
            <el-timeline>
              <el-timeline-item
                v-for="[key, value] in Object.entries(scope.row.beanLifeCycles)"
                :key="key"
                :timestamp="'耗时: ' + value.duration + '/ms'"
                icon="el-icon-bottom"
                type="primary">
                {{ key }}
              </el-timeline-item>
            </el-timeline>
          </div>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>

export default {
  name: "BeanCreationAnalysis",
  props: ['beanInitResultList'],
  mounted() {
    this.loading = false;
  },
  data() {
    return {
      loading: true,
      defaultSort: {prop: 'duration', order: 'descending'},
      search: {
        beanName: '',
        duration: 100
      },
      tableData: [],
      rowLevelStyle: [
        /* green */
        {background: '#7fff7f'},
        {background: '#c1ff84'},
        {background: '#c6ff8e'},
        {background: '#c9ff93'},
        {background: '#ccff99'},
        {background: '#ceff9e'},
        {background: '#d1ffa3'},
        {background: '#d3ffa8'},
        {background: '#d6ffad'},
        {background: '#d8ffb2'},

        /* warning */
        {background: '#ffffad'},
        {background: '#ffffa8'},
        {background: '#ffffa3'},
        {background: '#ffff9e'},
        {background: '#ffff99'},
        {background: '#ffff93'},
        {background: '#ffff8e'},
        {background: '#ffff89'},
        {background: '#ffff84'},
        {background: '#ffff7f'},

        /* error */
        {background: '#ffddbc'},
        {background: '#ffdbb7'},
        {background: '#ffd8b2'},
        {background: '#ffd6ad'},
        {background: '#ffd3a8'},
        {background: '#ffd1a3'},
        {background: '#ffce9e'},
        {background: '#ffcc99'},
        {background: '#ffc993'},
        {background: '#ffc68e'},
      ]
    };
  },

  methods: {

    getTableData() {
      return this.beanInitResultList.filter(data => {

        if (data.duration < this.search.duration) {
          return false;
        }

        return !this.search.beanName || (data.name.toLowerCase().includes(this.search.beanName.toLowerCase()));
      })
    },

    /**
     * 颜色太亮,  暂时关闭
     *
     * @param data
     * @returns {{}}
     */
    getRowStyle(data) {
      return {};
      //
      // let levels = this.rowLevelStyle.length;
      //
      // let currentLevel = Math.floor(data.row.duration / 10);
      //
      // if (currentLevel >= this.rowLevelStyle.length) {
      //   return this.rowLevelStyle[levels - 1];
      // } else {
      //   return this.rowLevelStyle[currentLevel - 1];
      // }

    }

  },

};
</script>

<style>
.bean-creation-analysis {
}

.el-table .cell {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.bean-name {
  border-left: 2px solid deepskyblue;
  margin-left: 3px;
  margin-right: 3px;
}

.text {
  font-size: 12px;
}

.item {
  margin-bottom: 5px;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both
}

.el-table tbody tr:hover > td {
  background-color: transparent !important;
}

</style>
