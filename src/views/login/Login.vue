<template>
  <div id="login-container">
    <div style="text-align: center; height: 50px">登录</div>
    <el-form
      :model="ruleForm"
      status-icon
      :rules="rules"
      ref="ruleForm"
      label-width="100px"
      class="demo-ruleForm"
    >
      <el-form-item label="账号" prop="user">
        <el-input
          type="text"
          v-model="ruleForm.user"
          autocomplete="off"
        ></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="pass">
        <el-input
          type="password"
          v-model="ruleForm.pass"
          autocomplete="off"
        ></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')"
          >提交</el-button
        >
        <el-button @click="resetForm('ruleForm')">重置</el-button>
        <el-button @click="toRegister">还未注册</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
// import axios from 'axios'
export default {
  name: "login",
  data() {
    var validatePass = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入账户"));
      } else {
        if (this.ruleForm.pass !== "") {
          this.$refs.ruleForm.validateField("pass");
        }
        callback();
      }
    };
    var validatePass2 = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("请输入密码"));
      } else {
        callback();
      }
    };
    return {
      ruleForm: {
        user: "",
        pass: "",
      },
      validate: {
        user: this.$route.params.user,
        psw: this.$route.params.psw,
      },
      rules: {
        user: [{ validator: validatePass, trigger: "blur", required: true }],
        pass: [{ validator: validatePass2, trigger: "blur", required: true }],
      },
      isLogin: false,
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          if (
            window.localStorage.getItem('user') == this.ruleForm.user &&
            window.localStorage.getItem('psw') == this.ruleForm.pass
          ) {
            this.$message.success("登录成功!!!");
            this.$router.push({ path: "/home" });
          } else {
            alert("用户名或密码错误");
          }
        } else {
          this.$message("请补全表单");
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    toRegister() {
      this.$router.push({
        name:'Register',
        params: {
          user: this.validate.user,
          psw: this.validate.psw,
        },
      });
    },
  },
};
</script>

<style scoped>
body {
  margin: 0;
}
#login-container {
  width: 400px;
  height: 290px;
  background: #e5e9f2;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -220px;
  margin-top: -170px;
  border-radius: 5px;
  padding-top: 40px;
  padding-right: 40px;
}
</style>
