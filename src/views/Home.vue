<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <el-row :gutter="20">
        <el-col :span="6" :offset="18">
          <el-input
            placeholder="Search..."
            v-model="searchText"
            @keyup.native.enter="searchBook"
            size="mini"
          >
            <el-button slot="append" icon="el-icon-search" @click="searchBook">
            </el-button>
          </el-input>
        </el-col>
      </el-row>
    </div>
    <el-table :data="tableData" style="width: 100%">
      <el-table-column
        prop="pkBookId"
        label="BookId"
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
        prop="amount"
        label="Amount"
        align="center"
      ></el-table-column>
      <el-table-column prop="handle" label="handle" align="center">
        <template slot-scope="scope">
          <el-button
            type="success"
            icon="el-icon-s-goods"
            circle
            :disabled="scope.row.amount === 0"
            @click="subscribe(scope.row)"
          ></el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-pagination
      @current-change="handleCurrentChange"
      background
      :current-page="currentPage"
      layout="prev, pager, next"
      :total="totalBooks"
      id="pageNav"
    >
    </el-pagination>
  </el-card>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      tableData: [],
      totalBooks: 10,
      currentPage: 1,
      searchText: ""
    };
  },
  methods: {
    getBookList(page, sum, searchText) {
      const _this = this;
      this.$axios
        .get(
          "http://localhost:8081/library/book/getBookList/" +
            page +
            "/" +
            sum +
            "/" +
            searchText
        )
        .then(function(result) {
          _this.tableData = result.data.info.BookList.records;
          _this.totalBooks = result.data.info.BookList.total;
        });
    },
    searchBook() {
      this.currentPage = 1;
      this.getBookList(1, 10, this.searchText);
    },
    handleCurrentChange(val) {
      this.currentPage = val;
      this.getBookList(val, 10, this.searchText);
    },
    subscribe(info) {
      console.log(info);
      if (!sessionStorage.getItem("userName")) {
        this.$message({
          type: "warning",
          message: "请先登录！"
        });
        return null;
      }
      const _this = this;
      this.$axios
        .post("http://localhost:8081/library/record/subscribe", {
          pkBookId: info.pkBookId,
          pkUserName: sessionStorage.getItem("userName"),
          pkBookName: info.pkBookName,
          author: info.author
        })
        .then(function(result) {
          console.log(result);
          if (result.data.code === 100) {
            _this.$message.success(result.data.info.subscribe);
          } else {
            _this.$message.error(result.data.info.subscribe);
          }
          _this.getBookList(_this.currentPage, 10, "");
        });
    }
  },
  created() {
    this.getBookList(1, 10, "");
  }
};
</script>

<style>
.box-card {
  margin-left: 50px;
  margin-right: 50px;
}
#pageNav {
  float: right;
  margin-top: 10px;
}
</style>
