<template>
  <view class="home-header">
    <image
      class="home-userImg"
      src="../../static/我的.png"
    ></image>
    <view class="search-container">
      <image
        class="home-search"
        src="../../static/搜索.png"
      ></image>
      <input
        class="home-input"
        type="text"
        placeholder="教练课程视频"
      />
    </view>
    <view class="action-buttons">
      <image
        class="home-calendar"
        src="../../static/日历.png"
      ></image>
      <view class="message-wrapper">
        <image
          class="home-message"
          src="../../static/信息.png"
        ></image>
        <view class="message-badge">2</view>
      </view>
    </view>
  </view>
</template>

<script>
export default {};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.home-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px;
  background-color: #ffffff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

.home-userImg {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.search-container {
  display: flex;
  align-items: center;
  background-color: #f5f5f5;
  border-radius: 20px;
  padding: 8px 15px;
  flex: 1;
  margin: 0 15px;
}

.home-search {
  width: 20px;
  height: 20px;
  margin-right: 8px;
  opacity: 0.6;
}

.home-input {
  border: none;
  background: transparent;
  flex: 1;
  font-size: 14px;
  color: #333;
}

.home-input::placeholder {
  color: #999;
}

.action-buttons {
  display: flex;
  gap: 15px;
}

.home-calendar,
.home-message {
  width: 24px;
  height: 24px;
  opacity: 0.8;
  transition: opacity 0.2s;
}

.home-calendar:active,
.home-message:active {
  opacity: 0.6;
}

.message-wrapper {
  position: relative;
}

.message-badge {
  position: absolute;
  top: -1vw;
  right: -1vw;
  background-color: #ff4757;
  color: white;
  font-size: 10px;
  min-width: 10px;
  height: 10px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2px;
  font-weight: 500;
  box-shadow: 0 1px 2px rgba(255, 71, 87, 0.3);
}
</style>