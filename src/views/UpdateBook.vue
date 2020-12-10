<template>
  <el-dialog
    title="UpdateBook"
    :visible.sync="dialogUpdateBookVisible"
    width="30%"
  >
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm">
      <el-form-item
        label="pkBookId"
        :label-width="formLabelWidth"
        prop="pkBookId"
      >
        <el-input v-model="ruleForm.pkBookId" disabled></el-input>
      </el-form-item>
      <el-form-item
        label="pkBookName"
        :label-width="formLabelWidth"
        prop="pkBookName"
      >
        <el-input v-model="ruleForm.pkBookName"></el-input>
      </el-form-item>
      <el-form-item label="author" :label-width="formLabelWidth" prop="author">
        <el-input v-model="ruleForm.author"></el-input>
      </el-form-item>
      <el-form-item label="amount" :label-width="formLabelWidth" prop="amount">
        <el-input v-model="ruleForm.amount"></el-input>
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
  name: "UpdateBook",
  data() {
    return {
      dialogUpdateBookVisible: false,
      formLabelWidth: "150px",
      ruleForm: {
        pkBookId: "",
        pkBookName: "",
        author: "",
        amount: ""
      },
      rules: {
        pkBookName: [
          { required: true, message: "请输入书名", trigger: "blur" }
        ],
        author: [{ required: true, message: "请输入作者名", trigger: "blur" }],
        amount: [{ required: true, message: "请输入库存", trigger: "blur" }]
      }
    };
  },
  methods: {
    open(info) {
      console.log(info);
      this.ruleForm.pkBookId = info.pkBookId;
      this.ruleForm.pkBookName = info.pkBookName;
      this.ruleForm.author = info.author;
      this.ruleForm.amount = info.amount;
      this.dialogUpdateBookVisible = true;
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
        .put("http://localhost:8081/library/book/updateBook", this.ruleForm)
        .then(function(result) {
          console.log(result.data);
          if (result.data.code === 200) {
            _this.$message.error(result.data.info.updateBookInfo);
            return;
          }
          _this.$message.success("更新成功");
          _this.dialogUpdateBookVisible = false;
          sessionStorage.setItem("activeName", "BookManage");
          _this.$router.go(0);
        });
    }
  }
};
</script>

<style scoped></style>
