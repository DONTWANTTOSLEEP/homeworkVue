<template>
  <el-tabs tab-position="left" v-model="activeName" @tab-click="handleTabClick">
    <!-- 管理员CRUD -->
    <el-tab-pane label="管理员管理" v-if="isSystem" name="AdminManage">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <el-row :gutter="40">
            <el-col :span="8" :offset="12">
              <el-input
                placeholder="Search..."
                v-model="searchText"
                @keyup.native.enter="searchUser"
                size="mini"
              >
                <el-button
                  slot="append"
                  icon="el-icon-search"
                  @click="searchUser"
                >
                </el-button>
              </el-input>
            </el-col>
            <el-col :span="4">
              <el-button
                type="primary"
                icon="el-icon-plus"
                size="mini"
                circle
                @click="addUserOpen"
              ></el-button>
            </el-col>
          </el-row>
        </div>
        <el-table :data="tableData" style="width: 100%">
          <el-table-column
            prop="pkId"
            label="pkId"
            align="center"
            width="100"
          ></el-table-column>
          <el-table-column
            prop="pkName"
            label="pkName"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="password"
            label="password"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="userLevel"
            label="userLevel"
            align="center"
          ></el-table-column>
          <el-table-column prop="handle" label="handle" align="center">
            <template slot-scope="scope">
              <el-button
                size="mini"
                @click="updateUserOpen(scope.row)"
                type="primary"
                >编辑</el-button
              >
              <el-button
                size="mini"
                type="danger"
                @click="handleDelete(scope.row)"
                >删除</el-button
              >
            </template>
          </el-table-column>
        </el-table>

        <el-pagination
          @current-change="handleCurrentChange"
          background
          :current-page="currentPage"
          layout="prev, pager, next"
          :total="totalUsers"
          id="pageNav"
        >
        </el-pagination>
      </el-card>
    </el-tab-pane>
    <!-- 用户CRUD -->
    <el-tab-pane label="用户管理" name="UserManage">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <el-row :gutter="40">
            <el-col :span="8" :offset="12">
              <el-input
                placeholder="Search..."
                v-model="searchText"
                @keyup.native.enter="searchUser"
                size="mini"
              >
                <el-button
                  slot="append"
                  icon="el-icon-search"
                  @click="searchUser"
                >
                </el-button>
              </el-input>
            </el-col>
            <el-col :span="4">
              <el-button
                type="primary"
                icon="el-icon-plus"
                size="mini"
                circle
                @click="addUserOpen"
              ></el-button>
            </el-col>
          </el-row>
        </div>
        <el-table :data="tableData" style="width: 100%">
          <el-table-column
            prop="pkId"
            label="pkId"
            align="center"
            width="100"
          ></el-table-column>
          <el-table-column
            prop="pkName"
            label="pkName"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="password"
            label="password"
            align="center"
          ></el-table-column>
          <el-table-column
            prop="userLevel"
            label="userLevel"
            align="center"
          ></el-table-column>
          <el-table-column prop="handle" label="handle" align="center">
            <template slot-scope="scope">
              <el-button
                size="mini"
                @click="updateUserOpen(scope.row)"
                type="primary"
                >编辑</el-button
              >
              <el-button
                size="mini"
                type="danger"
                @click="handleDelete(scope.row)"
                >删除</el-button
              >
            </template>
          </el-table-column>
        </el-table>

        <el-pagination
          @current-change="handleCurrentChange"
          background
          :current-page="currentPage"
          layout="prev, pager, next"
          :total="totalUsers"
          id="pageNav0"
        >
        </el-pagination>
      </el-card>
    </el-tab-pane>
    <UpdateUser ref="updateUserDialog"></UpdateUser>
    <AddUser ref="addUserDialog"></AddUser>
    <!-- 图书CRUD -->
    <el-tab-pane label="图书管理" name="BookManage">
      <el-card class="box-card">
        <div slot="header" class="clearfix">
          <el-row :gutter="40">
            <el-col :span="8" :offset="12">
              <el-input
                placeholder="Search..."
                v-model="searchText"
                @keyup.native.enter="searchBook"
                size="mini"
              >
                <el-button
                  slot="append"
                  icon="el-icon-search"
                  @click="searchBook"
                >
                </el-button>
              </el-input>
            </el-col>
            <el-col :span="4">
              <el-button
                type="primary"
                icon="el-icon-plus"
                size="mini"
                circle
                @click="addBookOpen"
              ></el-button>
            </el-col>
          </el-row>
        </div>
        <el-table :data="tableData" style="width: 100%">
          <el-table-column
            prop="pkBookId"
            label="BookId"
            align="center"
            width="100"
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
                size="mini"
                @click="updateBookOpen(scope.row)"
                type="primary"
                >编辑</el-button
              >
              <el-button
                size="mini"
                type="danger"
                @click="handleDeleteBook(scope.row)"
                >删除</el-button
              >
            </template>
          </el-table-column>
        </el-table>

        <el-pagination
          @current-change="handleCurrentChangeForBook"
          background
          :current-page="currentPage"
          layout="prev, pager, next"
          :total="totalBooks"
          id="pageNavBook"
        >
        </el-pagination>
      </el-card>
    </el-tab-pane>
    <AddBook ref="addBookDialog"></AddBook>
    <UpdateBook ref="updateBookDialog"></UpdateBook>
    <!-- 借还操作 -->
    <el-tab-pane label="借还管理" name="BorrowAndReturn">
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
          <el-table-column label="borrowState" align="center">
            <template slot-scope="scope">
              <!-- <el-button @click="test(scope)"></el-button> -->
              <el-tag
                effect="dark"
                type="success"
                v-if="scope.row.borrowState === 0"
                >申请借阅
              </el-tag>
              <el-tag
                effect="dark"
                type="warning"
                v-if="scope.row.borrowState === 2"
                >申请归还
              </el-tag>
            </template>
          </el-table-column>
          <el-table-column prop="handle" label="handle" align="center">
            <template slot-scope="scope">
              <!--              @click="function(scope.row)"-->
              <el-button size="mini" type="primary" @click="agree(scope.row)"
                >同意</el-button
              >
              <el-button
                size="mini"
                :disabled="scope.row.borrowState === 2"
                type="danger"
                @click="refuse(scope.row)"
                >拒绝</el-button
              >
            </template>
          </el-table-column>
        </el-table>

        <el-pagination
          @current-change="handleCurrentChangeForBorrow"
          background
          :current-page="currentPage"
          layout="prev, pager, next"
          :total="totalRecords"
          id="pageNavBorrow"
        >
        </el-pagination>
      </el-card>
    </el-tab-pane>
    <!-- 操作记录 -->
    <el-tab-pane label="操作记录" name="HandleRecord">
      <el-card class="box-card">
        <el-table
          ref="filterTable"
          :data="tableData"
          style="width: 100%"
          @filter-change="filterOperate"
        >
          <!-- opId -->
          <el-table-column
            prop="pkOpId"
            label="OpId"
            align="center"
            width="100"
          ></el-table-column>
          <!-- opName -->
          <el-table-column
            prop="opName"
            label="OpName"
            align="center"
            column-key="opName"
            :filters="opNameTableData"
            :filter-multiple="false"
            filter-placement="bottom-start"
          ></el-table-column>
          <!-- opType -->
          <el-table-column
            prop="opType"
            label="OpType"
            align="center"
            column-key="opType"
            :filters="opTypeTableData"
            :filter-multiple="false"
            filter-placement="bottom-start"
            ><template slot-scope="scope">
              <el-tag effect="dark" v-if="scope.row.opType === '同意'"
                >同意
              </el-tag>
              <el-tag
                effect="dark"
                type="danger"
                v-if="scope.row.opType === '拒绝'"
                >拒绝
              </el-tag>
            </template></el-table-column
          >
          <!-- opWho -->
          <el-table-column
            prop="opWho"
            label="OpWho"
            align="center"
            column-key="opWho"
            :filters="opWhoTableData"
            :filter-multiple="false"
            filter-placement="bottom-start"
          ></el-table-column>
          <!-- opState -->
          <el-table-column
            prop="opState"
            label="opState"
            align="center"
            column-key="opState"
            :filters="opStateTableData"
            :filter-multiple="false"
            filter-placement="bottom-start"
          >
            <template slot-scope="scope">
              <!-- <el-button @click="test(scope)"></el-button> -->
              <el-tag
                effect="dark"
                type="success"
                v-if="scope.row.opState === 0"
                >借阅申请
              </el-tag>
              <el-tag
                effect="dark"
                type="warning"
                v-if="scope.row.opState === 2"
                >归还申请
              </el-tag>
            </template>
          </el-table-column>
          <!-- opBook -->
          <el-table-column
            prop="opBook"
            label="OpBook"
            align="center"
            column-key="opBook"
            :filters="opBookTableData"
            :filter-multiple="false"
            filter-placement="bottom-start"
          ></el-table-column>
          <!-- opTime -->
          <el-table-column
            prop="opTime"
            label="OpTime"
            align="center"
          ></el-table-column>
        </el-table>

        <el-pagination
          @current-change="handleCurrentChangeForAdminRecord"
          background
          :current-page="currentPage"
          layout="prev, pager, next"
          :total="totalOperates"
          id="pageNavAdminRecord"
        >
        </el-pagination>
      </el-card>
    </el-tab-pane>
  </el-tabs>
</template>

<script>
import UpdateUser from "@/views/UpdateUser";
import AddUser from "@/views/AddUser";
import AddBook from "@/views/AddBook";
import UpdateBook from "@/views/UpdateBook";
export default {
  name: "Manage",
  components: { UpdateBook, AddBook, AddUser, UpdateUser },
  data() {
    return {
      isSystem: sessionStorage.getItem("isSystem"),
      isAdmin: sessionStorage.getItem("isAdmin"),
      //重写这个
      isFilterOperate: false,
      tableData: [],
      opNameTableData: [],
      opTypeTableData: [
        { text: "同意", value: "同意" },
        { text: "拒绝", value: "拒绝" }
      ],
      opStateTableData: [
        { text: "借阅申请", value: 0 },
        { text: "归还申请", value: 2 }
      ],
      opWhoTableData: [],
      opBookTableData: [],
      //待筛选的管理员名字
      opName: null,
      //待筛选的操作类型字段
      opType: null,
      //待筛选的用户名
      opWho: null,
      //待筛选的借阅状态
      opState: null,
      //待筛选的书名
      opBook: null,
      //json
      operateJson: {},
      searchText: "",
      currentPage: 1,
      totalUsers: 10,
      totalBooks: 10,
      totalRecords: 10,
      totalOperates: 10,
      level: 1,
      activeName: sessionStorage.getItem("activeName")
    };
  },
  methods: {
    handleTabClick(tab, event) {
      console.log("点击侧栏标签", tab, event);
      if (tab.name === "AdminManage") {
        this.currentPage = 1;
        this.searchText = "";
        this.level = 1;
        sessionStorage.setItem("activeName", "AdminManage");
        this.getUserList(this.level, 1, 10, "");
      }
      if (tab.name === "UserManage") {
        this.searchText = "";
        this.currentPage = 1;
        this.level = 0;
        sessionStorage.setItem("activeName", "UserManage");
        this.getUserList(this.level, 1, 10, "");
      }
      if (tab.name === "BookManage") {
        this.searchText = "";
        this.currentPage = 1;
        sessionStorage.setItem("activeName", "BookManage");
        this.getBookList(1, 10, "");
      }
      if (tab.name === "BorrowAndReturn") {
        this.currentPage = 1;
        sessionStorage.setItem("activeName", "BorrowAndReturn");
        this.getBorrowList(1, 10);
      }
      if (tab.name === "HandleRecord") {
        this.currentPage = 1;
        sessionStorage.setItem("activeName", "HandleRecord");
        this.getOperateList(1, 10);
        this.getOpCol("op_name");
        this.getOpCol("op_who");
        this.getOpCol("op_book");
        this.$refs.filterTable.clearFilter();
        this.isFilterOperate = false;
        this.operateJson = {};
        this.opName = null;
        this.opType = null;
        this.opWho = null;
        this.opState = null;
        this.opBook = null;
      }
    },
    handleDelete(info) {
      console.log(info);
      this.$confirm("是否删除 【 " + info.pkName + " 】", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          this.deleteUser(info.pkId);
          this.$router.go(0);
          // this.currentPage = 1;
          // this.getUserList(this.level, 1, 10, "");
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    handleDeleteBook(BookInfo) {
      console.log(BookInfo);
      this.$confirm("是否删除 【 " + BookInfo.pkBookName + " 】", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning"
      })
        .then(() => {
          this.deleteBook(BookInfo.pkBookId);
          this.$router.go(0);
          // this.currentPage = 1;
          // this.getBookList(1, 10, "");
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除"
          });
        });
    },
    handleCurrentChange(val) {
      this.currentPage = val;
      this.getUserList(this.level, val, 10, this.searchText);
    },
    handleCurrentChangeForBook(val) {
      this.currentPage = val;
      this.getBookList(val, 10, "");
    },
    handleCurrentChangeForBorrow(val) {
      this.currentPage = val;
      this.getBorrowList(val, 10, 2);
    },
    handleCurrentChangeForAdminRecord(val) {
      this.currentPage = val;
      if (!this.isFilterOperate) {
        this.getOperateList(val, 10);
      } else {
        this.getOperateListByOperateJson(val, 10);
      }
    },
    getUserList(userLevel, page, sum, searchText) {
      const _this = this;
      this.$axios
        .get(
          "http://localhost:8081/library/user/getUserByLevel/" +
            userLevel +
            "/" +
            page +
            "/" +
            sum +
            "/" +
            searchText
        )
        .then(function(result) {
          _this.tableData = result.data.info.UserInfo.records;
          _this.totalUsers = result.data.info.UserInfo.total;
        });
    },
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
    getBorrowList(page, sum) {
      const _this = this;
      this.$axios
        .get(
          "http://localhost:8081/library/record/getRecordByAdmin/" +
            page +
            "/" +
            sum
        )
        .then(function(result) {
          console.log(result);
          _this.tableData = result.data.info.recordsInfo.records;
          _this.totalRecords = result.data.info.recordsInfo.total;
        });
    },
    getOperateList(page, sum) {
      const _this = this;
      this.$axios
        .get(
          "http://localhost:8081/library/operate/getOpList/" + page + "/" + sum
        )
        .then(function(result) {
          // console.log("操作记录", result);
          _this.tableData = result.data.info.OpInfo.records;
          _this.totalOperates = result.data.info.OpInfo.total;
        });
    },
    getOperateListByOperateJson(page, sum) {
      const _this = this;
      this.$axios
        .post(
          "http://localhost:8081/library/operate/getOpListByOp/" +
            page +
            "/" +
            sum,
          this.operateJson
        )
        .then(function(result) {
          console.log("筛选结果", result);
          _this.tableData = result.data.info.OpInfo.records;
          _this.totalOperates = result.data.info.OpInfo.total;
        });
    },
    //根据列 获取待筛选列表
    getOpCol(col) {
      const _this = this;
      this.$axios
        .get("http://localhost:8081/library/operate/getOneOperate/" + col)
        .then(function(result) {
          // console.log("每一列的筛选列表", result);
          if (col === "op_name") {
            _this.opNameTableData = result.data.info.opCol;
          }
          if (col === "op_who") {
            _this.opWhoTableData = result.data.info.opCol;
          }
          if (col === "op_book") {
            _this.opBookTableData = result.data.info.opCol;
          }
        });
    },
    deleteUser(id) {
      const _this = this;
      this.$axios
        .delete("http://localhost:8081/library/user/deleteUserById/" + id)
        .then(function(result) {
          console.log(result.data);
          _this.returnInfo(result.data.code, "删除");
        });
    },
    deleteBook(id) {
      const _this = this;
      this.$axios
        .delete("http://localhost:8081/library/book/deleteBookById/" + id)
        .then(function(result) {
          console.log(result.data);
          _this.returnInfo(result.data.code, "删除");
        });
    },
    searchUser() {
      this.currentPage = 1;
      this.getUserList(this.level, 1, 10, this.searchText);
    },
    searchBook() {
      this.currentPage = 1;
      this.getBookList(1, 10, this.searchText);
    },
    returnInfo(code, msg) {
      if (code === 100) {
        this.$message({
          type: "success",
          message: msg + "成功"
        });
      } else {
        this.$message({
          type: "info",
          message: msg + "失败"
        });
      }
    },
    updateUserOpen(info) {
      this.$refs.updateUserDialog.open(info);
    },
    updateBookOpen(info) {
      this.$refs.updateBookDialog.open(info);
    },
    addUserOpen() {
      this.$refs.addUserDialog.open();
    },
    addBookOpen() {
      this.$refs.addBookDialog.open();
    },
    agree(info) {
      // console.log(info);
      const _this = this;
      this.$axios
        .put(
          "http://localhost:8081/library/record/updateRecord/" +
            sessionStorage.getItem("userName"),
          info
        )
        .then(function(result) {
          console.log(result);
          if (result.data.code === 100) {
            _this.$router.go(0);
            _this.$message.success(result.data.info.agreeInfo);
          } else {
            _this.$message.error(result.data.info.agreeInfo);
          }
        });
    },
    refuse(info) {
      // console.log(info);
      const _this = this;
      this.$axios
        .put(
          "http://localhost:8081/library/record/refuse/" +
            sessionStorage.getItem("userName"),
          info
        )
        .then(function(result) {
          console.log("拒绝申请", result);
          if (result.data.code === 100) {
            _this.$router.go(0);
            _this.$message.success(result.data.info.refuseInfo);
          } else {
            _this.$message.error(result.data.info.refuseInfo);
          }
        });
    },
    filterOperate(value) {
      if (Object.keys(value)[0] === "opName") {
        if (value.opName.length !== 0) {
          this.opName = value.opName[0];
        } else {
          this.opName = null;
        }
      }
      if (Object.keys(value)[0] === "opType") {
        if (value.opType.length !== 0) {
          this.opType = value.opType[0];
        } else {
          this.opType = null;
        }
      }
      if (Object.keys(value)[0] === "opWho") {
        if (value.opWho.length !== 0) {
          this.opWho = value.opWho[0];
        } else {
          this.opWho = null;
        }
      }
      if (Object.keys(value)[0] === "opState") {
        if (value.opState.length !== 0) {
          this.opState = value.opState[0];
        } else {
          this.opState = null;
        }
      }
      if (Object.keys(value)[0] === "opBook") {
        if (value.opBook.length !== 0) {
          this.opBook = value.opBook[0];
        } else {
          this.opBook = null;
        }
      }
      this.isFilterOperate = !(
        this.opName === null &&
        this.opType === null &&
        this.opWho === null &&
        this.opState === null &&
        this.opBook === null
      );
      this.operateJson = {
        opName: this.opName,
        opType: this.opType,
        opWho: this.opWho,
        opState: this.opState,
        opBook: this.opBook
      };
      console.log(
        "筛选",
        this.isFilterOperate,
        JSON.stringify(this.operateJson)
      );
      if (this.isFilterOperate) {
        this.currentPage = 1;
        this.getOperateListByOperateJson(1, 10);
      } else {
        this.isFilterOperate = false;
        this.currentPage = 1;
        this.getOperateList(1, 10);
      }
    }
  },
  created() {
    if (sessionStorage.getItem("activeName") === "AdminManage") {
      this.level = 1;
      this.getUserList(this.level, 1, 10, "");
    }
    if (sessionStorage.getItem("activeName") === "UserManage") {
      this.level = 0;
      this.getUserList(this.level, 1, 10, "");
    }
    if (sessionStorage.getItem("activeName") === "BookManage") {
      this.getBookList(1, 10, "");
    }
    if (sessionStorage.getItem("activeName") === "BorrowAndReturn") {
      this.getBorrowList(1, 10);
    }
    if (sessionStorage.getItem("activeName") === "HandleRecord") {
      this.getOperateList(1, 10);
      this.getOpCol("op_name");
      this.getOpCol("op_who");
      this.getOpCol("op_book");
    }
  }
};
</script>

<style>
#pageNav,
#pageNav0,
#pageNavBook,
#pageNavBorrow,
#pageNavAdminRecord {
  float: right;
  margin-top: 20px;
}
.el-table-filter {
  max-height: 300px;
  overflow: auto;
}
</style>
