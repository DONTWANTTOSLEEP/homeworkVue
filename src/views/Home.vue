<template>
  <div id="app">
    <!-- 外层容器 -->
    <el-container>
      <!-- 顶层容器 -->
      <el-header>
        <el-row>
          <!-- 标题 -->
          <el-col :span="5" id="navTitle">图书管理系统</el-col>
          <el-col :span="8" id="navButton" v-show="isUser">
            <el-menu
              :default-active="$route.path"
              class="el-menu-user"
              mode="horizontal"
              active-text-color="blue"
              v-bind:router="true"
            >
              <el-menu-item index="/borrowed">
                <i class="el-icon-edit"></i>
                Borrowed
              </el-menu-item>
              <el-menu-item index="/history">
                <i class="el-icon-s-data"></i>
                History
              </el-menu-item>
            </el-menu>
          </el-col>
          <el-col :span="7" :offset="12" v-show="!login">
            <el-button
              round
              icon="el-icon-ice-cream-round"
              class="button"
              @click="registerOpen"
              style="margin-top: 10px; float: right"
              >Register</el-button
            >
            <el-button
              round
              icon="el-icon-lollipop"
              class="button"
              style="margin-top: 10px; float: right"
              @click="loginOpen"
              >Login</el-button
            >
          </el-col>
          <el-dropdown
            id="user"
            @command="handleCommand"
            :show-timeout="100"
            v-show="login"
          >
            <span class="el-dropdown-link"
              >{{ name }}<i class="el-icon-arrow-down el-icon--right"></i
            ></span>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item command="manage" v-show="isAdmin || isSystem"
                >Manage</el-dropdown-item
              >
              <el-dropdown-item command="logout" divided
                >Logout</el-dropdown-item
              >
            </el-dropdown-menu>
          </el-dropdown>
        </el-row>
      </el-header>
      <!-- 主要区域容器 -->
      <el-main>
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
                  <el-button
                    slot="append"
                    icon="el-icon-search"
                    @click="searchBook"
                  >
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
      </el-main>
    </el-container>

    <register ref="registerDialog"></register>
    <login ref="loginDialog"></login>
  </div>
</template>

<script>
import register from "./Register";
import login from "./Login";

export default {
  name: "Home",
  components: {
    register,
    login
  },
  data() {
    return {
      isSystem: sessionStorage.getItem("isSystem"),
      isAdmin: sessionStorage.getItem("isAdmin"),
      isUser: sessionStorage.getItem("isUser"),
      name: sessionStorage.getItem("userName"),
      login: sessionStorage.getItem("userName"),
      tableData: [],
      totalBooks: 10,
      currentPage: 1,
      searchText: ""
    };
  },
  methods: {
    registerOpen() {
      this.$refs.registerDialog.open();
    },
    loginOpen() {
      this.$refs.loginDialog.open();
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
    handleCommand(command) {
      if (command === "logout") {
        sessionStorage.setItem("userName", "");
        sessionStorage.setItem("isSystem", "");
        sessionStorage.setItem("isAdmin", "");
        sessionStorage.setItem("isUser", "");
        this.login = 0;
        this.name = "";
        this.isAdmin = "";
        this.isSystem = "";
        this.isUser = "";
        this.$router.go(0);
      }
      if (command === "manage") {
        sessionStorage.setItem("activeName", "AdminManage");
        this.$router.push({
          name: "Manage"
        });
      }
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
      if (!this.login) {
        this.$message({
          type: "warning",
          message: "请先登录！"
        });
        return null;
      }
      this.$axios.post("http://localhost:8081/library/record/subscribe")
    }
  },
  created() {
    this.getBookList(1, 10, "");
  }
};
</script>

<style>
#app {
  font-family: Verdana, sans-serif;
}
#user {
  float: right;
  margin-top: 20px;
}
.box-card {
  margin-left: 50px;
  margin-right: 50px;
}
#pageNav {
  float: right;
  margin-top: 10px;
}
#navTitle {
  font-size: 30px;
  margin-top: 10px;
}
</style>
