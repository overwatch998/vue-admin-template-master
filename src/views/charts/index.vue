<template>
  <div class="chart-container">
    <el-row :span="20">
      <div class="chart-w">
        <max-chart />
      </div>
    </el-row>
    <el-row :gutter="20">
      <el-col :span="12">
        <div class="chart-w" style="margin:30px 15px 0 30px">
          <p class="worCloudTitle">患病职业词云</p>
          <word-cloud />
        </div>
      </el-col>
      <el-col :span="12">
        <div class="chart-w" style="margin:30px 30px 0 15px">
          <p class="worCloudTitle">患病职业柱状图</p>
          <pro-bar />
        </div>
      </el-col>
    </el-row>

    <el-row :gutter="20">
      <el-col :span="12">
        <div class="chart-w" style="margin:30px 15px 30px 30px">
          <p class="worCloudTitle">患病年龄分布图</p>
          <pie-chart />
        </div>
      </el-col>
      <el-col :span="12">
        <div class="chart-w" style="margin:30px 15px 30px 30px">
          <p class="worCloudTitle">患病月份折线图</p>
          <line-chart />
        </div>
      </el-col>
    </el-row>
  </div>
</template>
<script>
import RaddarChart from "./components/RaddarChart";
import PieChart from "./components/PieChart";
import BarChart from "./components/BarChart";
import MaxChart from "./components/MaxChart";
import WordCloud from "./components/WordCloud";
import ProBar from "./components/ProBar";
import LineChart from "./components/LineChart";
// 这里使用主题：macarons
require("echarts/theme/macarons");

export default {
  name: "KeyboardChart",
  components: {
    MaxChart,
    WordCloud,
    PieChart,
    ProBar,
    LineChart
  },
};
</script>

<style scoped>
.chart-container {
  position: relative;
  width: 100%;
  background-color: #f0f2f5;
  /* height: calc(100vh - 84px); */
}
.chart-w {
  background-color: #fff;
  position: relative;
  padding: 20px;
  margin: 30px 30px 0px 30px;
  border-radius: 5px;
}
.worCloudTitle{
  margin-top:0;
  position: relative;
  text-align: center;
  font-size:30px;
  color:#008acd;
}

/* .chart-w:hover {
  box-shadow: rgba(17, 17, 26, 0.1) 0px 8px 24px, rgba(17, 17, 26, 0.1) 0px 16px 20px, rgba(17, 17, 26, 0.1) 0px 24px 20px;
} */
</style>
