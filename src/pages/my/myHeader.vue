<template>
  <div class="my-header">
    <div class="user-info">
      <div class="avatar">
        <img src="@/static/我的.png" alt="用户头像">
      </div>
      <div class="info">
        <div class="username">{{userInfo.username}}</div>
        <div class="member-status">
          <span :class="['member-tag', userInfo.isMember ? 'vip-tag' : 'normal-tag']">
            {{userInfo.isMember ? 'VIP会员' : '普通会员'}}
          </span>
        </div>
      </div>
    </div>
    <div class="nav-arrow">
      <img src="@/static/右箭头.png" alt="导航箭头">
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyHeader',
  data() {
    return {
      userInfo: {
        username: uni.getStorageSync('username') || '用户名',
        isMember: uni.getStorageSync('isMember') || false
      }
    }
  },
  onShow() {
    // 每次显示组件时更新用户信息
    this.userInfo.username = uni.getStorageSync('username') || '用户名';
    this.userInfo.isMember = uni.getStorageSync('isMember') || false;
  }
}
</script>

<style scoped>
.my-header {
  padding: 16px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #ffffff;
  box-shadow: 0 2px 8px rgba(0,0,0,0.05);
}

.user-info {
  display: flex;
  align-items: center;
  gap: 16px;
}

.avatar {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  overflow: hidden;
  border: 2px solid #f0f0f0;
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}

.avatar img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.info {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.username {
  font-size: 20px;
  font-weight: 600;
  color: #2c3e50;
}

.member-status {
  display: flex;
  align-items: center;
}

.member-tag {
  padding: 4px 12px;
  border-radius: 15px;
  font-size: 12px;
  font-weight: 500;
}

.vip-tag {
  background: linear-gradient(135deg, #FFD700, #FFA500);
  color: #fff;
}

.normal-tag {
  background: #f5f5f5;
  color: #666;
  border: 1px solid #e0e0e0;
}

.nav-arrow {
  padding: 8px;
}

.nav-arrow img {
  width: 30px;
  height: 30px;
  opacity: 0.6;
}
</style>