<template>
  <el-dialog title="AddUser" :visible.sync="dialogAddUserVisible" width="30%">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm">
      <el-form-item label="pkName" :label-width="formLabelWidth" prop="pkName">
        <el-input v-model="ruleForm.pkName"></el-input>
      </el-form-item>
      <el-form-item
        label="password"
        :label-width="formLabelWidth"
        prop="password"
      >
        <el-input
          v-model="ruleForm.password"
          type="password"
          autocomplete="off"
        ></el-input>
      </el-form-item>
      <el-form-item
        label="userLevel"
        :label-width="formLabelWidth"
        prop="userLevel"
      >
        <el-select v-model="ruleForm.userLevel" placeholder="请选择">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
      </el-form-item>
    </el-form>

    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogAddUserVisible = false">Cancel</el-button>
      <el-button type="primary" @click="submitForm('ruleForm')">Add</el-button>
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: "AddUser",
  data() {
    return {
      dialogAddUserVisible: false,
      options: [
        {
          value: 1,
          label: "管理员"
        },
        {
          value: 0,
          label: "用户"
        }
      ],
      formLabelWidth: "150px",
      ruleForm: {
        pkName: "",
        password: "",
        userLevel: 0
      },
      rules: {
        pkName: [
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
      this.dialogAddUserVisible = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.Add();
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    Add() {
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
          if (_this.ruleForm.userLevel === 0) {
            sessionStorage.setItem("activeName", "UserManage");
          } else {
            sessionStorage.setItem("activeName", "AdminManage");
          }
          _this.$router.go(0);
        });
    }
  }
};
</script>

<style scoped></style>
