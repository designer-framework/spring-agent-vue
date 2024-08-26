<template>
  <div v-loading="loading" class="spring-analysis el-container">
    <el-container>
      <el-header height="100px">
        SpringAgent 应用启动耗时统计
      </el-header>
      <el-container>
        <el-main>
          <el-row :gutter="20">

            <el-col :span="24">
              <div class="grid-content bg-purple">
                <el-card class="box-card">
                  <div slot="header" class="clearfix">
                    <span class="card_title">SpringBoot基本信息</span>
                  </div>

                  <div>
                    <SpringApplicationInfo :statisticsList="tableData.statisticsList"/>
                  </div>
                </el-card>
              </div>
            </el-col>

            <el-col :span="24">
              <div class="grid-content bg-purple">
                <div>
                  <el-card class="box-card">
                    <div slot="header" class="clearfix">
                      <span class="card_title">Bean创建耗时统计</span>
                    </div>
                    <div>
                      <BeanCreationAnalysis :beanInitResultList="tableData.beanInitResultList"/>
                    </div>
                    <div>
                      <el-card class="box-card">
                      </el-card>
                    </div>
                  </el-card>
                </div>
              </div>
            </el-col>

            <el-col :span="24">
              <div class="grid-content bg-purple">
                <el-card class="box-card">
                  <div slot="header" class="clearfix">
                    <span class="card_title">高耗时组件方法统计图</span>
                  </div>
                  <div>
                    <ComponentsAnalysis v-if='tableData.componentsMetric.children'
                                        :componentsMetric='tableData.componentsMetric' style="height: 100vh"/>
                  </div>
                  <div>
                    <ComponentDetailAnalysis :componentsMetric="tableData.componentsMetric"/>
                  </div>
                </el-card>
              </div>
            </el-col>

            <el-col :span="24">
              <div class="grid-content bg-purple">
                <el-card class="box-card">
                  <div slot="header" class="clearfix">
                    <span class="card_title">类加载器加载Jar包统计</span>
                  </div>
                  <div>
                    <el-card class="box-card">
                      <ClassLoaderUnloadJarAnalysis :unusedJarMap="tableData.unusedJarMap"/>
                    </el-card>
                  </div>
                </el-card>
              </div>
            </el-col>

            <el-col :span="24">
              <div class="grid-content bg-purple">
                <el-card class="box-card">
                  <div slot="header" class="clearfix">
                    <span
                      class="card_title">方法调用耗时统计(包含<strong
                      style="color: #67c23a; font: 17px bold;">组件耗时统计</strong>和<strong
                      style="color: #67c23a; font: 17px bold;">Bean创建耗时统计</strong>及<strong
                      style="color: #67c23a; font: 17px bold;">配置文件中新增的插桩方法</strong>的全限定方法名, 可能存在重叠的耗时统计)</span>
                  </div>
                  <div>
                    <MethodInvokeAnalysis :methodInvokeDetailList="tableData.methodInvokeDetailList"/>
                  </div>
                </el-card>
              </div>
            </el-col>

            <el-col :span="24">
              <div class="grid-content bg-purple">
                <el-card class="box-card">
                  <div slot="header" class="clearfix">
                    <span class="card_title">性能火焰图</span>
                  </div>
                  <div>
                    <FlameGraphAnalysis/>
                  </div>
                </el-card>
              </div>
            </el-col>

          </el-row>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
import {$api} from "../common/utils/request";
import SpringApplicationInfo from "./SpringApplicationInfo.vue";
import ComponentsAnalysis from "./ComponentsAnalysis.vue";
import BeanCreationAnalysis from "./BeanCreationAnalysis.vue";
import MethodInvokeAnalysis from "./MethodInvokeAnalysis.vue";
import ComponentDetailAnalysis from "./ComponentDetailAnalysis.vue";
import ClassLoaderUnloadJarAnalysis from "./ClassLoaderUnloadJarAnalysis.vue";
import FlameGraphAnalysis from "./FlameGraphAnalysis.vue";

export default {
  name: 'SpringAnalysis',
  components: {
    SpringApplicationInfo,
    ComponentsAnalysis,
    BeanCreationAnalysis,
    MethodInvokeAnalysis,
    ComponentDetailAnalysis: ComponentDetailAnalysis,
    ClassLoaderUnloadJarAnalysis,
    FlameGraphAnalysis,
  },
  mounted() {
    this.getTableData()
    this.loading = false;
  },
  data() {
    return {
      title: 'title',
      like: true,
      activeName: 'method-invoke-analysis',
      loading: true,
      tableData: {
        'statisticsList': {},
        'componentsMetric': {},
        'beanInitResultList': [],
        'unusedJarMap': [],
        'methodInvokeDetailList': [],
      }
    };
  },
  methods: {
    async getTableData() {

      await $api('', (json) => {
        this.tableData = json;
      })

    }
  },

}
</script>

<style scoped>

.spring-analysis {
  height: 100%;
  width: 100%;
}

.el-header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  /* height: 160px; */
  background-color: #002140;
  color: #FFF;
  text-align: center;
  /* line-height: 60px; */
}

.bg-purple {
  margin-top: 30px;
}

.card_title {
  color: #1890ff;
}

body > .el-container {
  margin-bottom: 5px;
  height: 100%;
}
</style>
