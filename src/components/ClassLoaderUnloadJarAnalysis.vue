<template>
  <div class="class-loader-unload-jar-analysis">

    <el-table
      v-loading="loading"
      :data="this.unusedJarMap"
      :tree-props="{children: 'children', hasChildren: 'hasChildren'}"
      border
      element-loading-spinner="el-icon-loading"
      element-loading-text="拼命加载中"
      style="width: 100%;margin-bottom: 20px;">
      <el-table-column
        min-width="50%"
        type="expand"
        width="auto">

        <template slot-scope="scope">
          <div>
            <el-table
              :data="scope.row.children"
              border
              size="medium"
              style="width: 100%;">
              <el-table-column width="100%">
              </el-table-column>
              <el-table-column type="index">
              </el-table-column>
              <el-table-column
                label="Jar包路径"
                prop="name">
              </el-table-column>
            </el-table>
          </div>
        </template>

      </el-table-column>
      <el-table-column
        label="类加载器"
        prop="name"
        width="auto">
      </el-table-column>
      <el-table-column
        label="总JAR包数量"
        prop="allJarCount"
        width="auto">
      </el-table-column>
      <el-table-column
        label="已加载的JAR包数"
        prop="loadedJarCount"
        width="auto">
      </el-table-column>
      <el-table-column
        label="未加载的JAR包数"
        prop="unloadedJarCount"
        width="auto">
      </el-table-column>
    </el-table>

  </div>
</template>

<script>

export default {
  name: "ClassLoaderUnloadJarAnalysis",
  props: ['unusedJarMap'],
  mounted() {
    this.loading = false;
  },

  data() {
    return {
      loading: true,
    }
  },

  methods: {}

}
;
</script>

<style scoped>
.class-loader-unload-jar-analysis {
}

.el-table__body tbody tr:first-child td:nth-child(3) {
  width: 100%;
}
</style>
