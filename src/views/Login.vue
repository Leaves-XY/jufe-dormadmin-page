<template>
    <div class="login" >
        <div class="logo">
          <img
            src="https://leavesxy-web-tlias.oss-cn-hangzhou.aliyuncs.com/logo.png"
            alt=""
            style="margin-top:50px; width: 175px;height: 175px "
          >
        </div>
        <el-form
            :rules="rules"
            ref="loginForm"
            v-loading="loading"
            element-loading-text="正在登录..."
            element-loading-spinner="el-icon-loading"
            element-loading-background="rgba(0, 0, 0, 0.8)"
            :model="loginForm"
            class="loginContainer"
        >
            <h3 class="loginTitle">江西财经大学宿舍管理系统系统登录</h3>
            <el-form-item prop="username">
                <el-input
                    size="normal"
                    type="text"
                    v-model="loginForm.username"
                    auto-complete="off"
                    placeholder="请输入用户名"
                ></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <el-input
                    size="normal"
                    type="password"
                    v-model="loginForm.password"
                    auto-complete="off"
                    placeholder="请输入密码"
                ></el-input>
            </el-form-item>
            <el-form-item prop="code">
                <el-input
                    size="normal"
                    type="text"
                    v-model="loginForm.code"
                    auto-complete="off"
                    placeholder="点击图片更换验证码"
                    @keydown.enter.native="submitLogin"
                    style="width: 250px"
                ></el-input>
                <img
                    :src="vcUrl"
                    @click="updateVerifyCode"
                    alt=""
                    style="cursor: pointer"
                >
            </el-form-item>
            <el-button
                size="normal"
                type="primary"
                style="width: 100%;"
                @click="submitLogin"
            >登录</el-button>
          <el-button
            size="normal"
            type="success"
            style="width: 100%; margin-top: 10px;margin-left:0px "
            @click="goToRegister"
          >前往注册</el-button>
        </el-form>
    </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      loading: false,
      vcUrl: "/api/verifyCode?time=" + new Date(),
      loginForm: {
        username: "",
        password: "",
        code: ""
      },
      checked: true,
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" }
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
        code: [{ required: true, message: "请输入验证码", trigger: "blur" }]
      }
    };
  },
  methods: {
    updateVerifyCode() {
      this.vcUrl = "/api/verifyCode?time=" + new Date();
    },
    submitLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true;
          this.postRequest("/api/doLogin", this.loginForm).then(resp => {
            this.loading = false;
            if (resp) {
              this.$store.commit("INIT_CURRENTUSER", resp.obj);
              window.sessionStorage.setItem("user", JSON.stringify(resp.obj));
              let path = this.$route.query.redirect;
              this.$router.replace(
                path == "/" || path == undefined ? "/home" : path
              );
            } else {
              this.vcUrl = "/api/verifyCode?time=" + new Date();
            }
          });
        } else {
          return false;
        }
      });
    },
    goToRegister() {
      this.$router.push("/register");
    }
  }
};
</script>

<style>


.loginContainer {

  border-radius: 15px;
  background-clip: padding-box;
  margin: 80px auto;
  width: 350px;
  padding: 15px 35px 15px 35px;
  background: rgb(255,255,255,0.8);
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}

.loginTitle {
  margin: 15px auto 20px auto;
  text-align: center;
  color: #505458;
}

.loginRemember {
  text-align: left;
  margin: 0px 0px 15px 0px;
}
.el-form-item__content {
  display: flex;
  align-items: center;
}
.login {
  background-image: url("https://leavesxy-web-tlias.oss-cn-hangzhou.aliyuncs.com/login-bg-4.jpg");
  display: flex;
  align-items: center;
  flex-direction: column;
  background-size: cover;
  height: 100vh;
}
</style>
