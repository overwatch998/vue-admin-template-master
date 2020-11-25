<template>
  <div class="app-container">
    <el-backtop></el-backtop>
    <el-table
      border
      stripe
      style="min-width: 7%"
      fit
      highlight-current-row
      :data="list"
    >
      <!-- <el-table-column width="80px" align="center" label="排名年份" prop="rank_year">
        <template slot-scope="scope">
          <span>{{ scope.row.rank_year }}</span>
        </template>
      </el-table-column>-->
      <el-table-column sortable min-width="4%" align="center" label="id" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="7%" align="center" label="患者姓名" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.name }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="10%" align="center" label="患儿家长姓名" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.parent }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="10%" align="center" label="有效证件号" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.idnum }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="5%" align="center" label="性别" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.sex }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="7%" align="center" label="出生日期" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.birth }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="7%" align="center" label="年龄" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.age }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="10%" align="center" label="现住地址国标" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.idnation }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="7%" align="center" label="人群分类" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.profession }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="7%" align="center" label="发病日期" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.dateofaccident }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="7%" align="center" label="诊断时间" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.diagnosisoftime }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="7%" align="center" label="死亡日期" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.dateofdeath }}</span>
        </template>
      </el-table-column>
      <el-table-column sortable min-width="7%" align="center" label="疾病名称" prop="rank">
        <template slot-scope="scope">
          <span>{{ scope.row.disease }}</span>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
  background
  layout="prev, pager, next"
  :total="total"
  :current-page="currentPage"
  :page-size="pageSize"
  @current-change="handleCurrentChange"
  style="margin-top:20px"
  >
</el-pagination>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [],
      // total: 0,
      // page: 1,
      editDialogVisible: false,
      editform: {},
      search: "",
      currentPage: 1,
      total: 0,
      pageNo: 1,
      pageSize: 50
    };
  },
  created() {
    this.getList();
  },
  methods: {
    getList() {
      var vm = this;
      // this.$loading.show();
      this.axios
        .get("http://"+this.axiosURL+"/api/getall?pageNo="+vm.pageNo+"&pageSize="+vm.pageSize)
        .then(response => {
          // vm.total = response.data.total;
          vm.total = response.data.total;
          vm.list = response.data.list;
          vm.list.forEach(e => {
            e.birth = vm.formatDatetime(e.birth);
            e.diagnosisoftime = vm.formatDatetime(e.diagnosisoftime);
            e.dateofaccident = vm.formatDatetime(e.dateofaccident);
          });
          // this.$loading.hide();
          // vm.total = response.data.count;
          // vm.page = page;
        });
    },
    formatDatetime(str) {
      return str.slice(0, 10);
    },
    handleCurrentChange(val) {
     this.currentPage = val;
     this.pageNo = val;
     this.getList();
    },
  }
};
</script>

<style scoped>
.app-container{
  /* background-color: rgba(254, 220, 223, 0.336); */
}
.edit-input {
  padding-right: 100px;
}
.cancel-btn {
  position: absolute;
  right: 15px;
  top: 10px;
}
.pagination {
  margin-top: 20px;
  text-align: right;
}
.bold {
  font-weight: bold;
}
.onsell {
  background-color:#42cf90;
  border-color:#42cf90;
}
</style>