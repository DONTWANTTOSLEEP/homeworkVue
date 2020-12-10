<template>
  <el-dialog
    title="UpdateUser"
    :visible.sync="dialogUpdateUserVisible"
    width="30%"
  >
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm">
      <el-form-item label="pkId" :label-width="formLabelWidth" prop="pkId">
        <el-input v-model="ruleForm.pkId" disabled></el-input>
      </el-form-item>
      <el-form-item label="pkName" :label-width="formLabelWidth" prop="pkName">
        <el-input v-model="ruleForm.pkName"></el-input>
      </el-form-item>
      <el-form-item
        label="password"
        :label-width="formLabelWidth"
        prop="password"
      >
        <el-input v-model="ruleForm.password" autocomplete="off"></el-input>
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
      <el-button @click="dialogUpdateUserVisible = false">Cancel</el-button>
      <el-button type="primary" @click="submitForm('ruleForm')"
        >Update</el-button
      >
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: "UpdateUser",
  data() {
    return {
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
      dialogUpdateUserVisible: false,
      formLabelWidth: "150px",
      ruleForm: {
        pkId: "",
        pkName: "",
        password: "",
        userLevel: ""
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
    open(info) {
      console.log(info);
      this.ruleForm.pkId = info.pkId;
      this.ruleForm.pkName = info.pkName;
      this.ruleForm.password = info.password;
      this.ruleForm.userLevel = info.userLevel;
      this.dialogUpdateUserVisible = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.Update();
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    },
    Update() {
      console.log(this.ruleForm);
      const _this = this;
      this.$axios
        .put("http://localhost:8081/library/user/updateUser", this.ruleForm)
        .then(function(result) {
          console.log(result.data);
          if (result.data.code === 200) {
            _this.$message.error(result.data.info.updateUserInfo);
            return;
          }
          _this.$message.success("更新成功");
          _this.dialogUpdateUserVisible = false;
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
