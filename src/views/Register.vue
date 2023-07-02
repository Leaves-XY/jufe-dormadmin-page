<template>
  <div class="register">
    <div class="logo">
      <img
        src="../assets/logo.png"
        alt=""
        style="margin-top: 50px; width: 175px; height: 175px"
      >
    </div>
    <el-form
      :rules="rules"
      ref="registerForm"
      v-loading="loading"
      element-loading-text="正在注册..."
      element-loading-spinner="el-icon-loading"
      element-loading-background="rgba(0, 0, 0, 0.8)"
      :model="registerForm"
      class="registerContainer"
    >
      <h3 class="registerTitle">江西财经大学宿舍管理系统系统注册</h3>
      <el-form-item prop="username">
        <el-input
          size="normal"
          type="text"
          v-model="registerForm.username"
          auto-complete="off"
          placeholder="请输入用户名"
        ></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input
          size="normal"
          type="password"
          v-model="registerForm.password"
          auto-complete="off"
          placeholder="请输入密码"
        ></el-input>
      </el-form-item>
      <el-form-item prop="name">
        <el-input
          size="normal"
          type="text"
          v-model="registerForm.name"
          auto-complete="off"
          placeholder="请输入姓名"
        ></el-input>
      </el-form-item>
      <el-form-item prop="phone">
        <el-input
          size="normal"
          type="text"
          v-model="registerForm.phone"
          auto-complete="off"
          placeholder="请输入电话号码"
        ></el-input>
      </el-form-item>
      <el-form-item prop="remark">
        <el-select v-model="registerForm.remark" placeholder="请选择角色" clearable>
          <el-option value="寝室管理员" label="寝室管理员"></el-option>
          <el-option value="系统管理员" label="系统管理员"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="code">
        <el-input
          size="normal"
          type="text"
          v-model="registerForm.code"
          auto-complete="off"
          placeholder="点击图片更换验证码"
          @keydown.enter.native="submitRegister"
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
        @click="submitRegister"
      >注册</el-button>
      <el-button
        size="normal"
        type="success"
        style="width: 100%; margin-top: 10px;margin-left:0px "
        @click="goToLogin"
      >返回登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  name: "Register",
  data() {
    return {
      loading: false,
      vcUrl: "/api/verifyCode?time=" + new Date(),
      registerForm: {
        username: "",
        password: "",
        name: "",
        phone: "",
        remark: "寝室管理员",
        code: ""
      },
      rules: {
        username: [{ required: true, message: "请输入用户名", trigger: "blur" }],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
        name: [{ required: true, message: "请输入姓名", trigger: "blur" }],
        phone: [{ required: true, message: "请输入电话号码", trigger: "blur" }],
        remark: [{ required: true, message: "请选择角色", trigger: "blur" }],
        code: [{ required: true, message: "请输入验证码", trigger: "blur" }]
      }
    };
  },
  methods: {
    updateVerifyCode() {
      this.vcUrl = "/api/verifyCode?time=" + new Date();
    },
    submitRegister() {
      this.$refs.registerForm.validate(valid => {
        if (valid) {
          this.loading = true;
          this.postRequest("/api/user/register", this.registerForm).then(resp => {
            this.loading = false;6

            if (resp) {
              // 注册成功后的处理
            } else {
              this.vcUrl = "/api/verifyCode?time=" + new Date();
            }
          });
        } else {
          return false;
        }
      });
    },
    goToLogin() {
      this.$router.push("/login");
    }
  }
};
</script>

<style>
.registerContainer {
  border-radius: 15px;
  background-clip: padding-box;
  margin: 80px auto;
  width: 350px;
  padding: 15px 35px 15px 35px;
  background: rgb(255, 255, 255, 0.8);
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}

.registerTitle {
  margin: 15px auto 20px auto;
  text-align: center;
  color: #505458;
}

.registerRemember {
  text-align: left;
  margin: 0px 0px 15px 0px;
}

.el-form-item__content {
  display: flex;
  align-items: center;
}

.register {
  background-image: url("https://ssl.jxufe.edu.cn/cas/assets/images/default/login-bg.jpg");
  display: flex;
  align-items: center;
  flex-direction: column;
  background-size: cover;
  height: 100vh;
}
</style>
