<template>
  <div class="register-container">
    <div class="register-header">
      <div class="title-wrapper">
        <img
          src="../../static/KeepFitLogo.png"
          alt="logo"
          class="logo"
        >
        <h1 class="app-name">KeepFit</h1>
      </div>
      <h2 class="register-title">注册</h2>
    </div>

    <div class="register-form">
      <div class="form-item">
        <input
          type="text"
          v-model="registerForm.username"
          placeholder="请输入账号"
          style="width: 100%; padding-right: 15px;"
        />
      </div>
      <div class="form-item">
        <input
          v-show="showPassword"
          type="text"
          v-model="registerForm.password"
          autocomplete="new-password"
          placeholder="请输入密码"
        />
        <input
          v-show="!showPassword"
          v-model="registerForm.password"
          autocomplete="new-password"
          placeholder="请输入密码"
          password="true"
        />
        <div
          class="show-pwd"
          @click="togglePwdVisible"
        >
          <img
            :src="showPassword ? '../../static/眼睛打开.png' : '../../static/眼睛关闭.png'"
            :alt="showPassword ? '隐藏密码' : '显示密码'"
          >
        </div>
      </div>
      <div class="form-item">
        <input
          v-show="showConfirmPassword"
          type="text"
          v-model="registerForm.confirmPassword"
          autocomplete="new-password"
          placeholder="请确认密码"
        />
        <input
          v-show="!showConfirmPassword"
          v-model="registerForm.confirmPassword"
          autocomplete="new-password"
          placeholder="请确认密码"
          password="true"
        />
        <div
          class="show-pwd"
          @click="toggleConfirmPwdVisible"
        >
          <img
            :src="showConfirmPassword ? '../../static/眼睛打开.png' : '../../static/眼睛关闭.png'"
            :alt="showConfirmPassword ? '隐藏密码' : '显示密码'"
          >
        </div>
      </div>
      <button
        class="register-btn"
        @click="handleRegister"
      >注册</button>
    </div>

    <div class="agreement-text">
      <checkbox
        class="agreement-checkbox"
        :checked="isAgreed"
        @click="toggleAgreement"
        color="#6252dd"
      />
      注册即代表同意 <span class="link">用户协议</span> 和 <span class="link">隐私政策</span>
    </div>

    <div class="other-register">
      <div class="divider">
        <span>其他注册方式</span>
      </div>
      <div class="social-register">
        <img
          src="../../static/微信.png"
          alt="微信"
        >
        <img
          src="../../static/QQ.png"
          alt="QQ"
        >
        <img
          src="../../static/手机.png"
          alt="手机"
        >
      </div>
    </div>

    <div class="login-link">
      <span>已有账号</span>
      <span
        class="login"
        @click="goToLogin"
      >去登录</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "Register",
  data() {
    return {
      registerForm: {
        username: "",
        password: "",
        confirmPassword: "",
      },
      showPassword: false,
      showConfirmPassword: false,
      isAgreed: false,
    };
  },
  methods: {
    handleRegister() {
      if (!this.isAgreed) {
        uni.showToast({
          title: "请阅读并同意用户协议和隐私政策",
          icon: "none",
          duration: 2000,
        });
        return;
      }

      if (this.registerForm.username == "") {
        uni.showToast({
          title: "用户名不能为空",
          icon: "none",
        });
        return;
      }

      if (this.registerForm.password == "" || this.registerForm.confirmPassword == "") {
        uni.showToast({
          title: "密码不能为空",
          icon: "none",
        });
        return;
      }

      if (this.registerForm.password !== this.registerForm.confirmPassword) {
        uni.showToast({
          title: "两次密码不一致",
          icon: "none",
        });
        return;
      }

      const requestData = {
        userName: this.registerForm.username,
        passWord: this.registerForm.password,
        confirmPassword: this.registerForm.confirmPassword,
      };

      uni.request({
        url: "http://127.0.0.1:5000/api/auth/register",
        method: "POST",
        data: requestData,
        header: {
          "content-type": "application/json",
        },
        success: (res) => {
          if (res.statusCode === 200) {
            // 添加注册成功的提示
            uni.showToast({
              title: "注册成功",
              icon: "success",
              duration: 1500,
              success: () => {
                // 延迟跳转，等待提示显示完成
                setTimeout(() => {
                  uni.redirectTo({
                    url: "/pages/login/login",
                  });
                }, 1500);
              },
            });
          }
        },
        fail: (err) => {
          console.error(err);
          uni.showToast({
            title: "注册失败，请重试",
            icon: "none",
          });
        },
      });
    },
    togglePwdVisible() {
      this.showPassword = !this.showPassword;
    },
    toggleConfirmPwdVisible() {
      this.showConfirmPassword = !this.showConfirmPassword;
    },
    toggleAgreement() {
      this.isAgreed = !this.isAgreed;
    },
    goToLogin() {
      uni.navigateBack();
    },
  },
};
</script>

<style scoped>
.register-container {
  padding: 20px;
  min-height: 100vh;
  background: #fff;
}

.register-header {
  text-align: center;
  margin-bottom: 40px;
}

.title-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  margin-bottom: 10px;
}

.logo {
  width: 32px;
  height: 32px;
  border-radius: 20%;
}

.app-name {
  font-size: 28px;
  color: #333;
  margin: 0;
}

.register-title {
  font-size: 24px;
  color: #666;
}

.form-item {
  position: relative;
  margin-bottom: 20px;
  margin-right: 32px;
}

.form-item input {
  width: calc(100% - 30px); /* 减去左右padding的总和 */
  padding: 12px 15px;
  padding-right: 45px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  outline: none;
}

.show-pwd {
  position: absolute;
  right: -5%;
  top: 50%;
  transform: translateY(-50%);
  cursor: pointer;
  z-index: 2;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.show-pwd img {
  width: 20px;
  height: 20px;
  opacity: 0.6;
  pointer-events: none;
}

.register-btn {
  width: 100%;
  padding: 12px;
  background: #6252dd;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  margin-bottom: 15px;
}

.divider {
  text-align: center;
  margin: 20px 0 10px;
  color: #999;
}

.social-register {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 20px;
}

.social-register img {
  width: 40px;
  height: 40px;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 50%;
}

.login-link {
  text-align: center;
  margin-bottom: 20px;
  color: #666;
}

.login {
  color: #6252dd;
  margin-left: 5px;
}

.agreement-checkbox {
  transform: scale(0.8);
  margin-right: 5px;
}

.agreement-text {
  text-align: center;
  font-size: 15px;
  color: #999;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.agreement-text .link {
  color: #6252dd;
}
</style>