<template>
  <div class="method-invoke-analysis">

    <el-table
      v-loading="loading"
      :data="componentsMetric.children"
      :default-sort="defaultSort"
      :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
      border
      element-loading-spinner="el-icon-loading"
      element-loading-text="拼命加载中"
      size="medium"
      sort="percent"
      style="width: 100%">

      <el-table-column
        type="expand"
        width="100%">
        <template slot-scope="scope">
          <div>
            <el-table
              :data="scope.row.children"
              :default-sort="childTableSort"
              border
              size="medium"
              style="width: 100%;">
              <el-table-column width="100%">
              </el-table-column>
              <el-table-column type="index">
              </el-table-column>
              <el-table-column
                :show-overflow-tooltip="true"
                label="Bean名称/类名"
                prop="name">
              </el-table-column>

              <el-table-column
                label="耗时/ms"
                prop="value">
              </el-table-column>

              <el-table-column
                label="耗时占比"
                sortable>
                <template slot-scope="scope">
                  {{ scope.row.percent }}%
                </template>
              </el-table-column>
            </el-table>
          </div>
        </template>
      </el-table-column>

      <el-table-column
        label="耗时类型">
        <template slot-scope="scope">
          {{ scope.row.displayName }}
        </template>
      </el-table-column>

      <el-table-column
        label="耗时类名">
        <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
      </el-table-column>

      <el-table-column
        label="耗时/ms"
        prop="value">
      </el-table-column>

      <el-table-column
        label="耗时占比"
        sortable>
        <template slot-scope="scope">
          {{ scope.row.percent }}%
        </template>
      </el-table-column>
    </el-table>

  </div>
</template>

<script>

export default {
  name: "MethodInvokeAnalysis",
  props: ['componentsMetric'],
  mounted() {
    this.loading = false;
  },
  data() {
    return {
      loading: true,
      search: 100,
      defaultSort: {prop: 'totalCost', order: 'descending'},
      childTableSort: {prop: 'duration', order: 'descending'},
    }
  },

  methods: {}
};
</script>

<style scoped>
.method-invoke-analysis {
}
</style>
