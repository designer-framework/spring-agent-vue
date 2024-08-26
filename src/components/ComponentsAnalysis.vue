<template>
  <div id="components-analysis" v-loading="loading">
  </div>
</template>

<script>

export default {
  name: "ComponentsAnalysis",
  props: ['componentsMetric'],
  mounted() {
    this.initEcharts();
    this.loading = false;
  },
  //methods
  methods: {
    initEcharts() {
      //初始化echarts实例
      const echartsBI = this.$echarts.init(document.getElementById("components-analysis"));

      echartsBI.showLoading();

      //加载配置项和图表数据
      echartsBI.setOption(this.getOptions());

      window.onload = function () {
        echartsBI.resize();
      }

      echartsBI.hideLoading();

    },

    getSeriesItem(childComponent) {
      if (childComponent.children.length === 0) {
        return {
          name: childComponent.name,
          value: childComponent.value,
          desc: childComponent.desc
        };
      }
      let items = childComponent.children.map((componentItem) => {
        if (componentItem.name && componentItem.name.length > 55) {
          return (
            "{Line|..." + componentItem.name.slice(componentItem.name.length - 52, componentItem.name.length) + "             }{value|" + componentItem.value + "/ms}{rate|" + componentItem.percent + "%}"
          );
        } else {
          return (
            "{Line|" + componentItem.name + "             }{value|" + componentItem.value + "/ms}{rate|" + componentItem.percent + "%}"
          );
        }
      });

      return {
        name: childComponent.name,
        value: childComponent.value,
        desc: childComponent.desc,
        label: {
          show: true,
          rotate: 'tangential',
          formatter: [
            "{title|{b}}{abg|}",
            "{nameHead|ItemName}{valueHead|Duration}{rateHead|Percent}",
            "{hr|}",
            ...items,
          ].join("\n"),
          backgroundColor: '#F6F8FC',
          borderColor: '#8C8D8E',
          borderWidth: 1,
          borderRadius: 4,
          rich: {
            title: {
              color: '#eee',
              lineHeight: 17,
              align: 'center'
            },
            abg: {
              backgroundColor: '#333',
              width: "100%",
              align: "right",
              height: 17,
              borderRadius: [2, 2, 0, 0],
            },
            Line: {
              color: '#4C5058',
              width: '100%',
              align: "left",
            },
            nameHead: {
              color: "#333",
              height: 17,
              align: 'left'
            },
            hr: {
              backgroundColor: "#333",
              borderColor: '#8C8D8E',
              width: '100%',
              borderWidth: 1,
              height: 0,
              align: 'left'
            },
            value: {
              width: 20,
              padding: [0, 20, 0, 30],
              align: "right",
            },
            valueHead: {
              color: "#333",
              width: 20,
              padding: [0, 20, 0, 30],
              align: "right",
            },
            rate: {
              color: '#fff',
              backgroundColor: '#4C5058',
              padding: [3, 4],
              width: 40,
              align: "right",
              borderRadius: 4
            },
            rateHead: {
              color: "#333",
              width: 40,
              align: "right",
              padding: [0, 10, 0, 0],
            },
          },
        },
      };
    },


    getOptions() {
      return {
        legend: {
          type: 'scroll', //滚动
          //orient: 'vertical', //Title摆放位置
          center: 1,
          bottom: 20,
          emphasis: {
            selectorLabel: {
              lineHeight: 200
            }
          },
          itemStyle: {
            borderType: [5, 10],
            borderDashOffset: 5,
            opacity: 0.4
          },
          //legend
          data: this.componentsMetric.children.map((child) => {
            return child.name;
          }),
        },

        //data
        series: [
          {
            type: "pie",
            roseType: 'area',
            //radius: ['45%', '60%'],
            center: ["50%", "50%"],
            selectedMode: "single",
            avoidLabelOverlap: true,
            data: this.componentsMetric.children.map((childComponent) => {
              return this.getSeriesItem(childComponent);
            }),
            labelLine: {
              length: 12
            },
            startAngle: 1,
            emphasis: {
              itemStyle: {
                type: 'dashed',
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: "rgba(0, 0, 0, 0.5)",
              },
            }
          },
        ],
        title: {
          //text: "Spring Application Components Statistics",
          //subtext: "Fake Data",
          //left: "center",
        },
        tooltip: {
          trigger: "item",
          //formatter: "{a} <br/>{b} : {c} ({d}%)",
          formatter: function (item, ticket, callback) {
            return item.marker + ' ' + item.name + ': ' + item.value + ' (' + item.percent + '%)<br>' + (item.data.desc ? item.data.desc : '');
          }
        },
      };

    },

  },
  //data
  data() {
    return {
      loading: true,
      option: this.getOptions(),
    };
  },


};
</script>

<style scoped>
#components-analysis {
  height: 100%;
  width: 100%
}
</style>
