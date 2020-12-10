<template>
  <div id="app">
    <el-container>
      <!-- 顶层容器 -->
      <el-header>
        <el-row>
          <!-- 标题 -->
          <el-col :span="5" id="navTitle">图书管理系统</el-col>
          <el-col :span="9" id="navButton">
            <el-menu
              :default-active="$route.path"
              class="el-menu-user"
              mode="horizontal"
              active-text-color="blue"
              v-bind:router="true"
            >
              <el-menu-item index="/">
                <i class="el-icon-edit"></i>
                Home</el-menu-item
              >
              <el-menu-item index="/borrowed" v-show="login">
                <i class="el-icon-edit"></i>
                Borrowed
              </el-menu-item>
              <el-menu-item index="/history" v-show="login">
                <i class="el-icon-s-data"></i>
                History
              </el-menu-item>
            </el-menu>
          </el-col>
          <el-col :span="7" :offset="3" v-show="!login">
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
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
    <register ref="registerDialog"></register>
    <login ref="loginDialog"></login>
  </div>
</template>

<script>
import register from "@/views/Register";
import login from "@/views/Login";

export default {
  name: "app",
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
      login: sessionStorage.getItem("userName")
    };
  },
  methods: {
    registerOpen() {
      this.$refs.registerDialog.open();
    },
    loginOpen() {
      this.$refs.loginDialog.open();
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
    }
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
#navTitle {
  font-size: 30px;
  margin-top: 10px;
}
</style>
