<template>
  <el-dialog title="AddBook" :visible.sync="dialogAddBookVisible" width="30%">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm">
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
      <el-button @click="dialogAddUserVisible = false">Cancel</el-button>
      <el-button type="primary" @click="submitForm('ruleForm')">Add</el-button>
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: "AddBook",
  data() {
    return {
      dialogAddBookVisible: false,
      formLabelWidth: "150px",
      ruleForm: {
        pkBookName: "",
        author: "",
        amount: ""
      },
      rules: {
        pkBookName: [
          { required: true, message: "请输入书名", trigger: "blur" }
        ],
        author: [{ required: true, message: "请输入作者", trigger: "blur" }],
        amount: [{ required: true, message: "请输入库存", trigger: "blur" }]
      }
    };
  },
  methods: {
    open() {
      this.dialogAddBookVisible = true;
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
        .post("http://localhost:8081/library/book/addBook", this.ruleForm)
        .then(function(result) {
          console.log(result.data);
          if (result.data.code === 200) {
            _this.$message.error(result.data.info.addBookInfo);
            return;
          }
          _this.$message.success(result.data.info.addBookInfo);
          _this.dialogRegisterVisible = false;
          _this.$router.go(0);
          sessionStorage.setItem("activeName", "BookManage");
        });
    }
  }
};
</script>

<style scoped></style>
