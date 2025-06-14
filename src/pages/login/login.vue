<template>
  <div class="login-container">
    <div class="login-header">
      <div class="title-wrapper">
        <img
          src="../../static/KeepFitLogo.png"
          alt="logo"
          class="logo"
        >
        <h1 class="app-name">KeepFit</h1>
      </div>
      <h2 class="login-title">登录</h2>
    </div>

    <div class="login-form">
      <div class="form-item">
        <input
          type="text"
          v-model="loginForm.username"
          placeholder="账号"
          style="width: 100%; padding-right: 15px;"
        />
      </div>
      <div class="form-item">
        <input
          v-show="showPassword"
          type="text"
          v-model="loginForm.password"
          autocomplete="new-password"
          placeholder="密码"
        />
        <input
          v-show="!showPassword"
          v-model="loginForm.password"
          autocomplete="new-password"
          placeholder="密码"
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
      <button
        class="login-btn"
        @click="handleLogin"
      >登录</button>
      <div class="forget-pwd">
        <span @click="forgetPassword">忘记密码</span>
      </div>
    </div>

    <div class="agreement-text">
      <checkbox
        class="agreement-checkbox"
        :checked="isAgreed"
        @click="toggleAgreement"
        color="#6252dd"
      />
      登录即代表同意 <span class="link">用户协议</span> 和 <span class="link">隐私政策</span>
    </div>

    <div class="other-login">
      <div class="divider">
        <span>其他登录方式</span>
      </div>
      <div class="social-login">
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

    <div class="signup-link">
      <span>没有账号</span>
      <span
        class="signup"
        @click="goToSignup"
      >去注册</span>
    </div>

    <div class="guest-login">
      <button
        class="guest-btn"
        @click="loginAsGuest"
      >访客登录</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
      },
      showPassword: false,
      isAgreed: false,
    };
  },
  methods: {
    handleLogin() {
      if (!this.isAgreed) {
        uni.showToast({
          title: "请阅读并同意用户协议和隐私政策",
          icon: "none",
          duration: 2000,
        });
        return;
      }

      if (this.loginForm.username.trim() === "") {
        uni.showToast({
          title: "用户名不能为空",
          icon: "none",
        });
        return;
      }

      if (this.loginForm.password.trim() === "") {
        uni.showToast({
          title: "密码不能为空",
          icon: "none",
        });
        return;
      }

      const requestData = {
        userName: this.loginForm.username,
        passWord: this.loginForm.password,
      };

      uni.request({
        url: "http://127.0.0.1:5000/api/auth/login", // 你的接口地址
        method: "POST",
        data: requestData,
        header: {
          "content-type": "application/json",
        },
        success: (res) => {
          if (res.statusCode === 200) {
            // 保存用户名和VIP状态到本地存储
            uni.setStorageSync('username', this.loginForm.username);
            // 注意这里将isVip=="1"转换为false(不是vip)，isVip=="0"转换为true(是vip)
            uni.setStorageSync('isMember', res.data.isVip === "0");
            
            uni.showToast({
              title: "登录成功",
              icon: "success",
              duration: 1500,
              success: () => {
                setTimeout(() => {
                  uni.switchTab({
                    url: "/pages/home/home",
                  });
                }, 1500);
              },
            });
          } else {
            // 输出错误信息
            uni.showToast({
              title: res.data.message || "登录失败，请重试",
              icon: "none",
            });
          }
        },
        fail: (err) => {
          console.error(err);
          uni.showToast({
            title: "登录失败，请重试",
            icon: "none",
          });
        },
      });
    },
    togglePwdVisible() {
      this.showPassword = !this.showPassword;
    },
    forgetPassword() {
      console.log("忘记密码");
    },
    toggleAgreement() {
      this.isAgreed = !this.isAgreed;
    },
    goToSignup() {
      console.log("去注册");
      uni.navigateTo({
        url: "/pages/register/register",
      });
    },
    loginAsGuest() {
      // 设置访客信息
      uni.setStorageSync('username', '用户名');
      uni.setStorageSync('isMember', false);
      
      uni.switchTab({
        url: "/pages/home/home",
      });
    },
  },
};
</script>

<style scoped>
.login-container {
  padding: 20px;
  min-height: 100vh;
  background: #fff;
}

.login-header {
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
  margin: 0; /* 移除原有的 margin */
}

.login-title {
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
  cursor: pointer; /* 添加鼠标手型 */
  z-index: 2; /* 提高层级 */
  width: 30px; /* 增加可点击区域 */
  height: 30px; /* 增加可点击区域 */
  display: flex; /* 居中图标 */
  align-items: center;
  justify-content: center;
}

.show-pwd img {
  width: 20px;
  height: 20px;
  opacity: 0.6;
  pointer-events: none; /* 防止图片干扰点击事件 */
}

.login-btn {
  width: 100%;
  padding: 12px;
  background: #6252dd;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  margin-bottom: 15px;
}

.forget-pwd {
  text-align: right;
  color: #6252dd;
  margin-bottom: 10px;
}

.divider {
  text-align: center;
  margin: 20px 0 10px;
  color: #999;
}

.social-login {
  display: flex;
  justify-content: center;
  gap: 20px;
  margin-bottom: 20px;
}

.social-login img {
  width: 40px;
  height: 40px;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 50%;
}

.signup-link {
  text-align: center;
  margin-bottom: 20px;
  color: #666;
}

.signup {
  color: #6252dd;
  margin-left: 5px;
}

.guest-btn {
  width: 100%;
  padding: 12px;
  background: #f5f5f5;
  color: #666;
  border: none;
  border-radius: 8px;
  font-size: 14px;
}

.agreement-text {
  text-align: center;
  font-size: 15px;
  color: #999;
  display: flex;
  align-items: center;
  justify-content: center;
}

.agreement-checkbox {
  transform: scale(0.8);
  margin-right: 5px;
  accent-color: #6252dd;
}

.agreement-text .link {
  color: #6252dd;
}
</style>