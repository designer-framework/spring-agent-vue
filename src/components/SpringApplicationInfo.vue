<template>
  <div class="spring-boot-info">
    <el-row :gutter="20">
      <el-col v-for="item in statisticsList" :key="item.label" :span="8">
        <div>
          <el-statistic :title="getTitle(item)">
            <template slot="formatter">
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
</style>
