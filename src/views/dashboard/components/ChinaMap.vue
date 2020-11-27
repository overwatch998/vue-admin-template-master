<template>
  <div :class="className" :style="{ height: height, width: width }" />
</template>

<script>
import echarts from "echarts";
require("echarts/theme/macarons"); // echarts theme
import resize from "./mixins/resize";
import { axiosURL } from "@/settings";
import BMap from "BMap";
require("echarts/extension/bmap/bmap");

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
      default: "80vh",
    },
  },
  data() {
    return {
      chart: null,
      geoCoordMap: {
      },
      data: [
       
      ],
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
    convertData(ddata) {
      var res = [];
      for (var i = 0; i < ddata.length; i++) {
        var geoCoord = this.geoCoordMap[ddata[i].name];
        if (geoCoord) {
          res.push({
            name: ddata[i].name,
            value: geoCoord.concat(ddata[i].value),
          });
        }
      }
      return res;
    },
    initChart() {
      let vm = this;
      this.chart = echarts.init(this.$el, "macarons");
      this.chart.showLoading();
      //   this.axios.get("http://"+this.axiosURL+"/api/findByMonth").then((res) => {
      this.axios.get("http://localhost:8001/api/getMapData").then((res) => {
        let data1 = res.data;
        let areasList = data1.areas;
        let longList = data1.longtitudes;
        let latList = data1.latitudes;
        let numList = data1.nums;

        for (let i=0;i<areasList.length;i++) {
            vm.data.push({name:areasList[i], value:numList[i]});
            let geoList = [];
            geoList.push(longList[i]);
            geoList.push(latList[i]);
            vm.geoCoordMap[areasList[i]] = [longList[i], latList[i]];
        }

        console.log(vm.geoCoordMap);
        console.log(vm.data);

        this.chart.hideLoading();

        this.chart.setOption({
          title: {
            text: "全国患病城市（区）分布 - 百度地图",
            // subtext: "data from PM25.in",
            // sublink: "http://www.pm25.in",
            left: "center",
          },
          tooltip: {
            trigger: "item",
          },
          bmap: {
            center: [114.114129, 37.550339],
            zoom: 6,
            roam: true,
            mapStyle: {
              styleJson: [
                {
                  featureType: "water",
                  elementType: "all",
                  stylers: {
                    color: "#d1d1d1",
                  },
                },
                {
                  featureType: "land",
                  elementType: "all",
                  stylers: {
                    color: "#f3f3f3",
                  },
                },
                {
                  featureType: "railway",
                  elementType: "all",
                  stylers: {
                    visibility: "off",
                  },
                },
                {
                  featureType: "highway",
                  elementType: "all",
                  stylers: {
                    color: "#fdfdfd",
                  },
                },
                {
                  featureType: "highway",
                  elementType: "labels",
                  stylers: {
                    visibility: "off",
                  },
                },
                {
                  featureType: "arterial",
                  elementType: "geometry",
                  stylers: {
                    color: "#fefefe",
                  },
                },
                {
                  featureType: "arterial",
                  elementType: "geometry.fill",
                  stylers: {
                    color: "#fefefe",
                  },
                },
                {
                  featureType: "poi",
                  elementType: "all",
                  stylers: {
                    visibility: "off",
                  },
                },
                {
                  featureType: "green",
                  elementType: "all",
                  stylers: {
                    visibility: "off",
                  },
                },
                {
                  featureType: "subway",
                  elementType: "all",
                  stylers: {
                    visibility: "off",
                  },
                },
                {
                  featureType: "manmade",
                  elementType: "all",
                  stylers: {
                    color: "#d1d1d1",
                  },
                },
                {
                  featureType: "local",
                  elementType: "all",
                  stylers: {
                    color: "#d1d1d1",
                  },
                },
                {
                  featureType: "arterial",
                  elementType: "labels",
                  stylers: {
                    visibility: "off",
                  },
                },
                {
                  featureType: "boundary",
                  elementType: "all",
                  stylers: {
                    color: "#fefefe",
                  },
                },
                {
                  featureType: "building",
                  elementType: "all",
                  stylers: {
                    color: "#d1d1d1",
                  },
                },
                {
                  featureType: "label",
                  elementType: "labels.text.fill",
                  stylers: {
                    color: "#999999",
                  },
                },
              ],
            },
          },
          series: [
            {
              name: "患病",
              type: "scatter",
              coordinateSystem: "bmap",
              data: vm.convertData(vm.data),
              symbolSize: function (val) {
                return val[2]/3;
              },
              encode: {
                value: 2,
              },
              label: {
                formatter: "{b}",
                position: "right",
                show: false,
              },
              itemStyle: {
                color: "rgb(214,111,214)",
              },
              emphasis: {
                label: {
                  show: true,
                },
              },
            },
            {
              name: "Top 20",
              type: "effectScatter",
              coordinateSystem: "bmap",
              data: vm.convertData(
                vm.data
                  .sort(function (a, b) {
                    return b.value - a.value;
                  })
                  .slice(0, 20)
              ),
              symbolSize: function (val) {
                return val[2]/2;
              },
              encode: {
                value: 2,
              },
              showEffectOn: "render",
              rippleEffect: {
                brushType: "stroke",
              },
              hoverAnimation: true,
              label: {
                formatter: "{b}",
                position: "right",
                show: true,
              },
              itemStyle: {
                color: "purple",
                shadowBlur: 10,
                shadowColor: "#333",
              },
              zlevel: 1,
            },
          ],
        });
      });
    },
  },
};
</script>
