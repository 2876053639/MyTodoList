<template>
  <div id="register-container">
    <div style="text-align: center; height: 50px">注册</div>
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
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="ruleForm.email"></el-input>
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
        <el-button @click="toLogin">已注册用户，去登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "register",
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
    var validateEmail = (rule, value, callback) => {
      let mailReg = /^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(.[a-zA-Z0-9_-])+/;
      if (mailReg.test(value)) {
        callback();
      } else {
        callback(new Error("请输入正确的邮箱格式"));
      }
    };
    return {
      ruleForm: {
        user: "",
        pass: "",
        email: "",
      },
      validate: {
        test: "",
        user:this.$route.params.user,
        psw:this.$route.params.psw,
      },
      rules: {
        user: [{ validator: validatePass, trigger: "blur", required: true }],
        pass: [{ validator: validatePass2, trigger: "blur", required: true }],
        email: [
          {
            validator: validateEmail,
            trigger: "blur",
            required: true,
            type: "email",
            message: "请输入正确的邮箱格式",
          },
        ],
      },
      isLogin: false,
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          // this.$message.success("注册成功!!!");
          let test = this.ruleForm.email;
          // let test = JSON.stringify(this.ruleForm.email);
          //服务器ip:39.106.66.132
          axios({
            method: "POST",
            baseURL: "http://39.106.66.132:3434",
            url: "/index/getcode",
            params: {
              email: test,
            },
          }).then((res) => {
            this.validate.test = res.data.code;
            // this.$bus.$emit("getTest",this.validate.test);
            this.$router.push({
              name: "Validation",
              params: {
                test: this.validate.test,
                user:this.ruleForm.user,
                psw:this.ruleForm.pass,
              },
            });
          });
        } else {
          this.$message("请补全表单数据！");
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    toLogin() {
      this.$router.push({ name:'Login' ,params:{
        user:this.ruleForm.user,
        psw:this.ruleForm.pass,
      }});
    },
  },
};
</script>

<style scoped>
body {
  margin: 0;
}
#register-container {
  width: 450px;
  height: 310px;
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
