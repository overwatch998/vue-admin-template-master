<template>
  <div :class="className" :style="{ height: height, width: width }" />
</template>

<script>
import echarts from "echarts";
require("echarts/theme/macarons"); // echarts theme
import resize from "./mixins/resize";

const animationDuration = 6000;

export default {
  mixins: [resize],
  props: {
    className: {
      type: String,
      default: "chart",
    },
    width: {
      type: String,
      default: "100%",
    },
    height: {
      type: String,
      default: "30vh",
    },
  },
  data() {
    return {
      chart: null,
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart();
    });
  },
  beforeDestroy() {
    if (!this.chart) {
      return;
    }
    this.chart.dispose();
    this.chart = null;
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, "macarons");
      this.chart.showLoading();
      this.axios.get("http://"+this.axiosURL+"/api/findByPro").then((res) => {
        let data1 = res.data;
        // let list = [];
        let dataAxis = [], data = [];
        let maxNum = 0;
        for (let key in data1) {
          let proName = key;
          let num = data1[key];
          if (num > 5) {
          dataAxis.push(proName);
          data.push(num);
          if (num > maxNum) maxNum = num;
          }
        }
        console.log(dataAxis);
        console.log(data)
       
        var yMax = maxNum;
        var dataShadow = [];

        for (var i = 0; i < data.length; i++) {
          dataShadow.push(yMax);
        }
        this.chart.hideLoading();
        // this.myChart.hideLoading();
        this.chart.setOption({
          // title: {
          //   text: "患病职业分布柱状图",
          //   subtext: "滑动缩放",
          //   left:"center",
          //   textStyle: {
          //     fontSize: 30
          //   }
          // },
           toolbox: {
            show: true,
            feature: {
              dataView: { show: true, readOnly: false },
              magicType: { show: true, type: ["line", "bar"] },
              restore: { show: true },
              saveAsImage: { show: true },
            },
          },
          tooltip: {
            trigger: "axis",
          },
          xAxis: {
            data: dataAxis,
            axisLabel: {
              inside: false,
              rotate: -45,
              textStyle: {
                color: "black",
              },
            },
            axisTick: {
              show: false,
            },
            axisLine: {
              show: false,
            },
            z: 10,
          },
          yAxis: {
            axisLine: {
              show: false,
            },
            axisTick: {
              show: false,
            },
            axisLabel: {
              textStyle: {
                color: "#999",
              },
            },
          },
          dataZoom: [
            {
              type: "inside",
            },
          ],
          series: [
            {
              // For shadow
              type: "bar",
              itemStyle: {
                color: "rgba(0,0,0,0.05)",
              },
              barGap: "-100%",
              barCategoryGap: "40%",
              data: dataShadow,
              animation: false,
            },
            {
              type: "bar",
              itemStyle: {
                color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: "#2ec7c9" },
                  { offset: 0.5, color: "#2ec7c97a" },
                  { offset: 1, color: "#b6a2de" },
                ]),
              },
              emphasis: {
                itemStyle: {
                  color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
                    { offset: 0, color: "#b6a2de" },
                    { offset: 0.7, color: "#2ec7c97a" },
                    { offset: 1, color: "#2ec7c9" },
                  ]),
                },
              },
              data: data,
            },
          ],
        });
      });
    },
  },
};
</script>
