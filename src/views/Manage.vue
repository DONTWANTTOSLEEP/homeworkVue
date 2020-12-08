<template>
  <el-tabs tab-position="left" v-model="activeName" @tab-click="handleTabClick">
    <!-- 用户名，退出 -->
    <el-dropdown id="user" @command="handleCommand" :show-timeout="100">
      <span class="el-dropdown-link"
        >{{ userName }}<i class="el-icon-arrow-down el-icon--right"></i
      ></span>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item command="logout">Logout</el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
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
          <el-table-column prop="pkId" label="pkId"></el-table-column>
          <el-table-column prop="pkName" label="pkName"></el-table-column>
          <el-table-column prop="password" label="password"></el-table-column>
          <el-table-column prop="userLevel" label="userLevel"></el-table-column>
          <el-table-column prop="handle" label="handle">
            <template slot-scope="scope">
              <el-button size="mini" @click="updateUserOpen(scope.row)"
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
          <el-table-column prop="pkId" label="pkId"></el-table-column>
          <el-table-column prop="pkName" label="pkName"></el-table-column>
          <el-table-column prop="password" label="password"></el-table-column>
          <el-table-column prop="userLevel" label="userLevel"></el-table-column>
          <el-table-column prop="handle" label="handle">
            <template slot-scope="scope">
              <el-button size="mini" @click="updateUserOpen(scope.row)"
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
          <el-table-column prop="pkBookId" label="BookId"></el-table-column>
          <el-table-column prop="pkBookName" label="BookName"></el-table-column>
          <el-table-column prop="author" label="Author"></el-table-column>
          <el-table-column prop="amount" label="Amount"></el-table-column>
          <el-table-column prop="handle" label="handle">
            <template slot-scope="scope">
              <el-button size="mini" @click="updateBookOpen(scope.row)"
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
    <!-- 借还操作 -->
    <el-tab-pane label="借还管理" name="BorrowAndReturn">
      <el-card class="box-card">
        <el-table :data="tableData" style="width: 100%">
          <el-table-column prop="pkBookId" label="BookId"></el-table-column>
          <el-table-column prop="pkUserName" label="UserName"></el-table-column>
          <el-table-column prop="pkBookName" label="BookName"></el-table-column>
          <el-table-column prop="author" label="Author"></el-table-column>
          <el-table-column
            prop="borrowTime"
            label="borrowTime"
          ></el-table-column>
          <el-table-column
            prop="returnTime"
            label="returnTime"
          ></el-table-column>
          <el-table-column
            prop="borrowState"
            label="borrowState"
          ></el-table-column>
          <el-table-column prop="handle" label="handle">
            <!--            <template slot-scope="scope">-->
            <template>
              <!--              @click="function(scope.row)"-->
              <el-button size="mini">同意</el-button>
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
  </el-tabs>
</template>

<script>
import UpdateUser from "@/views/UpdateUser";
import AddUser from "@/views/AddUser";
import AddBook from "@/views/AddBook";
export default {
  name: "Manage",
  components: { AddBook, AddUser, UpdateUser },
  data() {
    return {
      userName: sessionStorage.getItem("userName"),
      isSystem: sessionStorage.getItem("isSystem"),
      isAdmin: sessionStorage.getItem("isAdmin"),
      tableData: [],
      searchText: "",
      currentPage: 1,
      totalUsers: 10,
      totalBooks: 10,
      totalRecords: 10,
      level: 1,
      activeName: sessionStorage.getItem("activeName")
    };
  },
  methods: {
    handleCommand(command) {
      if (command === "logout") {
        sessionStorage.setItem("userName", "");
        sessionStorage.setItem("isSystem", "");
        sessionStorage.setItem("isAdmin", "");
        sessionStorage.setItem("isUser", "");
        this.userName = "";
        this.isAdmin = "";
        this.isSystem = "";
        this.$router.push("/");
      }
    },
    handleTabClick(tab, event) {
      console.log(tab, event);
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
        this.searchText = "";
        this.currentPage = 1;
        sessionStorage.setItem("activeName", "BorrowAndReturn");
        this.getBorrowList(1, 10);
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
          "http://localhost:8081/library/record/getRecordList/" +
            page +
            "/" +
            sum
        )
        .then(function(result) {
          console.log(result);
          _this.tableData = result.data.info.recordsInfo.records;
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
    addUserOpen() {
      this.$refs.addUserDialog.open();
    },
    addBookOpen() {
      this.$refs.addBookDialog.open();
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
  }
};
</script>

<style scoped>
#user {
  float: right;
  text-align: center;
}
#pageNav0,
#pageNavBook,
#pageNavBorrow {
  float: right;
  margin-top: 20px;
}
</style>
