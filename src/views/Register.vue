<template>
  <el-dialog title="Register" :visible.sync="dialogRegisterVisible" width="30%">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm">
      <el-form-item
        label="Username"
        :label-width="formLabelWidth"
        prop="pkName"
      >
        <el-input v-model="ruleForm.pkName" autocomplete="off"></el-input>
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
        ></el-input>
      </el-form-item>
      <el-form-item
        label="PasswordAgain"
        :label-width="formLabelWidth"
        prop="passwordAgain"
      >
        <el-input
          type="password"
          v-model="ruleForm.passwordAgain"
          autocomplete="off"
          @keyup.native.enter="Register"
          show-password
        ></el-input>
      </el-form-item>
    </el-form>

    <div slot="footer" class="dialog-footer">
      <el-button @click="resetForm('ruleForm')">Reset</el-button>
      <el-button type="primary" @click="submitForm('ruleForm')"
        >Register</el-button
      >
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: "Register",
  data() {
    const validatePass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入密码"));
      } else {
        if (this.ruleForm.passwordAgain !== "") {
          this.$refs.ruleForm.validateField("passwordAgain");
        }
        callback();
      }
    };
    const validatePass2 = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请再次输入密码"));
      } else if (value !== this.ruleForm.password) {
        callback(new Error("两次输入密码不一致!"));
      } else {
        callback();
      }
    };
    return {
      dialogRegisterVisible: false,
      ruleForm: {
        pkName: "",
        password: "",
        passwordAgain: ""
      },
      formLabelWidth: "150px",
      rules: {
        pkName: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 14, message: "长度在 3 到 14 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, validator: validatePass, trigger: "blur" },
          { min: 6, max: 15, message: "长度在 6 到 15 个字符", trigger: "blur" }
        ],
        passwordAgain: [
          { required: true, validator: validatePass2, trigger: "blur" },
          { min: 6, max: 15, message: "长度在 6 到 15 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    open() {
      this.dialogRegisterVisible = true;
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.Register();
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    Register() {
      console.log(this.ruleForm);
      const _this = this;
      this.$axios
        .post("http://localhost:8081/library/user/register", this.ruleForm)
        .then(function(result) {
          console.log(result.data);
          if (result.data.code === 200) {
            _this.$message.error(result.data.info.registerInfo);
            return;
          }
          _this.$message.success(result.data.info.registerInfo);
          _this.dialogRegisterVisible = false;
          _this.$router.go(0);
        });
    }
  }
};
</script>

<style scoped></style>
