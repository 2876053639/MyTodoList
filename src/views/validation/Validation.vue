<template>
  <div id="validation">
    <div class="validation-container">验证码已发送至您的邮箱，请进行验证：</div>
    <el-form
      :model="ruleForm"
      status-icon
      :rules="rules"
      ref="ruleForm"
      label-width="100px"
      class="demo-ruleForm"
    >
      <el-form-item label="验证码" prop="validation">
        <el-input
          type="text"
          v-model="ruleForm.validation"
          autocomplete="off"
        ></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')"
          >确认</el-button
        >
        <el-button @click="toLogin">返回</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  name: "validation",
  data() {
    var validationText = (rule, value, callback) => {
      // console.log(value)
      if (value === "") {
        callback(new Error("请输入验证码"));
      } else {
        callback();
      }
    };
    return {
      ruleForm: {
        validation: "",
        test: this.$route.params.test,
        user: this.$route.params.user,
        psw: this.$route.params.psw,
      },

      rules: {
        validation: [
          { validator: validationText, trigger: "blur", required: true },
        ],
      },
      isLogin: false,
    };
  },
  methods: {
    toLogin() {
      this.$router.push({
        name: "Login",
        params: {
          user: this.ruleForm.user,
          psw: this.ruleForm.psw,
        },
      });
    },
    // getTest(test){
    //   this.ruleForm.test = test;
    //   console.log(test)
    // },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          if (this.ruleForm.test == this.ruleForm.validation) {
            this.$message.success("注册成功");
            this.$router.push({
              name: "Login",
              // params: {
              //   user: this.ruleForm.user,
              //   psw: this.ruleForm.psw,
              // },
            });
            window.localStorage.setItem('user',this.ruleForm.user);
            window.localStorage.setItem('psw',this.ruleForm.psw);
          } else {
            alert("验证码错误");
          }
        } else {
          this.$message("请填写验证码！");
        }
      });
    },
  },
  mounted() {
    // this.$bus.$on("getTest", this.getTest);
  },
};
</script>

<style scoped>
#validation {
  width: 300px;
  height: 200px;
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

.validation-container {
  margin: 0 auto;
  width: 290px;
  text-align: center;
  margin-left: 30px;
  margin-bottom: 20px;
}
</style>