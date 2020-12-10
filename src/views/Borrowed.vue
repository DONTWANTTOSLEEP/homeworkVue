<template>
  <el-card class="box-card">
    <el-table :data="tableData" style="width: 100%">
      <el-table-column
        prop="pkBookId"
        label="BookId"
        align="center"
        width="100"
      ></el-table-column>
      <el-table-column
        prop="pkUserName"
        label="UserName"
        align="center"
      ></el-table-column>
      <el-table-column
        prop="pkBookName"
        label="BookName"
        align="center"
      ></el-table-column>
      <el-table-column
        prop="author"
        label="Author"
        align="center"
      ></el-table-column>
      <el-table-column
        prop="borrowTime"
        label="borrowTime"
        align="center"
      ></el-table-column>
      <el-table-column label="state" align="center">
        <template slot-scope="scope">
          <!-- <el-button @click="test(scope)"></el-button> -->
          <el-tag
            effect="dark"
            v-if="scope.row.borrowState === 1"
            size="mini"
            @click="returnBook(scope.row)"
            style="cursor:pointer"
            >借阅中,点击申请归还
          </el-tag>
          <el-tag
            effect="dark"
            v-else-if="scope.row.borrowState === -1"
            size="mini"
            type="danger"
            >被管理员拒绝
          </el-tag>
          <el-tag type="warning" effect="dark" v-else size="mini"
            >等待管理员同意借阅申请
          </el-tag>
        </template>
      </el-table-column>
    </el-table>

    <el-pagination
      @current-change="handleCurrentChange"
      background
      :current-page="currentPage"
      layout="prev, pager, next"
      :total="totalRecords"
      id="pageNav"
    >
    </el-pagination>
  </el-card>
</template>

<script>
export default {
  name: "Borrowed",
  data() {
    return {
      tableData: [],
      totalRecords: 10,
      currentPage: 1
    };
  },
  methods: {
    getRecordList(page, sum, borrowState) {
      const _this = this;
      this.$axios
        .get(
          "http://localhost:8081/library/record/getRecordListByUser/" +
            page +
            "/" +
            sum +
            "/" +
            borrowState +
            "/" +
            sessionStorage.getItem("userName")
        )
        .then(function(result) {
          _this.tableData = result.data.info.recordsInfo.records;
          _this.totalRecords = result.data.info.recordsInfo.total;
        });
    },
    handleCurrentChange(val) {
      this.currentPage = val;
      this.getRecordList(val, 10, 0);
    },
    returnBook(info) {
      // console.log(info);
      const _this = this;
      this.$axios
        .put("http://localhost:8081/library/record/returnBook", info)
        .then(function(result) {
          console.log(result);
          if (result.data.code === 100) {
            _this.$router.go(0);
            _this.$message.success(result.data.info.returnInfo);
          } else {
            _this.$message.success(result.data.info.returnInfo);
          }
        });
    }
  },
  created() {
    this.getRecordList(1, 10, 0);
  }
};
</script>

<style scoped>
.box-card {
  margin-left: 10px;
  margin-right: 10px;
}
#pageNav {
  float: right;
  margin-top: 10px;
}
</style>
