<template>
  <el-dialog title="Login" :visible.sync="dialogLoginVisible" width="25%">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm">
      <el-form-item
        label="Username"
        :label-width="formLabelWidth"
        prop="username"
      >
        <el-input v-model="ruleForm.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item
        label="Password"
        :label-width="formLabelWidth"
        prop="password"
      >
        <el-input
          type="password"
          v-model="ruleForm.password"
          autocomplete="off"
          @keyup.native.enter="Login"
        ></el-input>
      </el-form-item>
    </el-form>

    <div slot="footer" class="dialog-footer">
      <el-button @click="resetForm('ruleForm')">Reset</el-button>
      <el-button type="primary" @click="submitForm('ruleForm')"
        >Login</el-button
      >
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      dialogLoginVisible: false,
      ruleForm: {
        username: "",
        password: ""
      },
      formLabelWidth: "120px",
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 14, message: "长度在 3 到 14 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入密码", trigger: "blur" },
          { min: 6, max: 15, message: "长度在 6 到 15 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    open() {
      this.dialogLoginVisible = true;
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.Login();
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    Login() {
      const _this = this;
      this.$axios
        .post("http://localhost:8081/library/user/login", {
          pkName: this.ruleForm.username,
          password: this.ruleForm.password
        })
        .then(function(result) {
          console.log(result.data);
          if (result.data.code === 200) {
            _this.$message.error(result.data.info.userInfo);
            return;
          }
          _this.$message.success("登录成功！");
          if (result.data.info.userInfo.userLevel === 0) {
            sessionStorage.setItem("isUser", "1");
          }
          if (result.data.info.userInfo.userLevel === 1) {
            sessionStorage.setItem("isAdmin", "1");
          }
          if (result.data.info.userInfo.userLevel === 2) {
            sessionStorage.setItem("isSystem", "1");
          }
          sessionStorage.setItem("userName", _this.ruleForm.username);

          console.log(sessionStorage);
          _this.dialogLoginVisible = false;
          _this.$router.go(0);
        });
    }
  }
};
</script>

<style scoped></style>
