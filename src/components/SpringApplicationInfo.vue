<template>
  <div class="spring-boot-info">
    <el-row :gutter="20">
      <el-col v-for="item in statisticsList" :key="item.label" :span="8" class="el-col-box">
        <div class="el-statistic-top">
          <el-statistic :title="getTitle(item)" >
            <template slot="prefix">
              {{ item.value }}
            </template>
          </el-statistic>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>

import ClassLoaderUnloadJarAnalysis from "./ClassLoaderUnloadJarAnalysis.vue";

export default {
  components: {ClassLoaderUnloadJarAnalysis},
  props: ['statisticsList'],
  name: "SpringApplicationInfo",
  mounted() {
  },
  data() {
    return {}
  },

  methods: {
    getTitle(item) {
      return "🎉" + item.label + "🎉";
    }
  }

}
;
</script>

<style scoped>
.spring-boot-info {
}

.like {
  cursor: pointer;
  font-size: 25px;
  display: inline-block;
}

.el-col-box {
  box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04);
  border-radius: 8px;
}

.el-statistic-top {
  margin: 8px;
}
</style>
