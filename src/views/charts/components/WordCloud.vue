<template>
  <div
    :class="className"
    :style="{ height: height, width: width }"
  />
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
    // marginTop: {
    //   type: String,
    //   default: "10px",
    // },
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
        let data = res.data;
        let list = [];
        for (let key in data) {
          let book_name = key;
          let comment_nums = data[key];
          list.push({ name: book_name, value: comment_nums });
        }
        console.log(list);
         this.chart.hideLoading();
        // this.myChart.hideLoading();
        this.chart.setOption({
          backgroundColor: "#fff",
          tooltip: {
            pointFormat: "{series.name}: <b>{point.percentage:.1f}%</b>",
          },
          series: [
            {
              type: "wordCloud",
              gridSize: 2,
              shape: "circle",
              // shape: 'pentagon',
              sizeRange: [30, 300],
              //   rotationRange: [-90, 90],
              left: "center",
              top: "center",
              width: "100%",
              height: "100%",
              right: null,
              bottom: null,
              drawOutOfBound: false,
              textStyle: {
                normal: {
                  fontFamily: "sans-serif",
                  fontWeight: "bold",
                  color: function () {
                    return (
                      "rgb(" +
                      [
                        Math.round(Math.random() * 160),
                        Math.round(Math.random() * 160),
                        Math.round(Math.random() * 160),
                      ].join(",") +
                      ")"
                    );
                  },
                },
                emphasis: {
                  shadowBlur: 10,
                  shadowColor: "#333",
                },
              },
              data: list,
            },
          ],
        });
      });
    },
  },
};
</script>
