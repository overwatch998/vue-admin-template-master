<template>
  <div :class="className" :style="{ height: height, width: width }" />
</template>

<script>
import echarts from "echarts";
require("echarts/theme/macarons"); // echarts theme
import resize from "./mixins/resize";

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
      default: "28vh",
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
      this.axios.get("http://"+this.axiosURL+"/api/findByDate").then((res) => {
        let data = res.data;
        let list = [];
        for (let key in data) {
          let book_name = key;
          let comment_nums = data[key];
          list.push({ name: book_name, value: comment_nums });
        }
         this.chart.hideLoading();

        this.chart.setOption({
          tooltip: {
            trigger: "item",
            formatter: "{a} <br/>{b} : {c} ({d}%)",
          },

        // title: {
        //   text: "各年份患病人数",
        //   // subtext: "纯属虚构",
        //   left: 'center',
        //   textStyle:
        //   {fontSize:50}
        // },
          legend: {
            show: false,
            type:"scroll",
            left:0,
            data: list.name,
          },
                     toolbox: {
            show: true,
            feature: {
              dataView: { show: true, readOnly: false },
              magicType: { show: true, type: ["pie"] },
              restore: { show: true },
              saveAsImage: { show: true },
            },
          },
          series: [
            {
              name: "各年份患病人数",
              type: "pie",
              roseType: "radius",
              radius: [30, 160],
              center: ["50%", "50%"],
              data: list,
              animationEasing: "cubicInOut",
              animationDuration: 3600,
              top:'100',
            },
          ],
        });
      });
    },
  },
};
</script>
