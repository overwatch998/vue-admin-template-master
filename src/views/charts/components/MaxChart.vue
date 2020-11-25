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
      default: "50vh",
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
      this.axios.get("http://"+this.axiosURL+"/api/findByAge").then((res) => {
        let data = res.data;
        let manList = data.man;
        let womanList = data.woman;
        this.chart.hideLoading();
        this.chart.setOption({
          title: {
            text: "各年龄段男女患病人数",
            // subtext: "纯属虚构",
            left: "center",
            textStyle: {
              fontSize: 50,
            },
          },
          tooltip: {
            trigger: "axis",
          },
          legend: {
            data: ["男性", "女性"],
            left: "0",
          },
          toolbox: {
            show: true,
            feature: {
              dataView: { show: true, readOnly: false },
              magicType: { show: true, type: ["line", "bar"] },
              restore: { show: true },
              saveAsImage: { show: true },
            },
          },
          calculable: true,
          xAxis: [
            {
              type: "category",
              data: [
                "0~10",
                "10~20",
                "20~30",
                "30~40",
                "40~50",
                "50~60",
                "60~70",
                "70~80",
                "80~90",
                "90~100",
              ],
            },
          ],
          yAxis: [
            {
              type: "value",
            },
          ],
          series: [
            {
              name: "男性",
              type: "bar",
              data: manList,
              // markPoint: {
              //   data: [
              //     { type: "max", name: "最大值" },
              //     { type: "min", name: "最小值" },
              //   ],
              // },
              markLine: {
                data: [{ type: "average", name: "平均值" }],
              },
            },
            {
              name: "女性",
              type: "bar",
              data: womanList,
              // markPoint: {
              //   data: [
              //     { name: "年最高", value: 182.2, xAxis: 7, yAxis: 183 },
              //     { name: "年最低", value: 2.3, xAxis: 11, yAxis: 3 },
              //   ],
              // },
              markLine: {
                data: [{ type: "average", name: "平均值" }],
              },
            },
          ],
        });
      });
    },
  },
};
</script>
