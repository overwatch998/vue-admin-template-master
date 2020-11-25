<template>
  <div :class="className" :style="{ height: height, width: width }" />
</template>

<script>
import echarts from "echarts";
require("echarts/theme/macarons"); // echarts theme
import resize from "./mixins/resize";
import { axiosURL } from '@/settings';

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
      this.axios.get("http://"+this.axiosURL+"/api/findByMonth").then((res) => {
        let data1 = res.data;
        let dataAxis = [], data = [];
        let maxNum = 0;
        for (let key in data1) {
          let proName = key;
          let num = data1[key];
          dataAxis.push(proName);
          data.push(num);
        }
        this.chart.hideLoading();

        this.chart.setOption({
                     toolbox: {
            show: true,
            feature: {
              dataView: { show: true, readOnly: false },
              magicType: { show: true, type: ["line", "bar"] },
              restore: { show: true },
              saveAsImage: { show: true },
            },
          },
          xAxis: {
            type: "category",
            data: ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov", "Dec"],
          },
          yAxis: {
            type: "value",
          },
          series: [
            {
              data: data,
              type: "line",
            },
          ],
        });
      });
    },
  },
};
</script>
