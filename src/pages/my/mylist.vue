<template>
  <div class="my-list">
    <div
      v-for="(item, index) in listItems"
      :key="index"
      class="list-item"
      :class="{ 'logout': item.type === 'logout' }"
      @click="handleClick(item.type)"
    >
      <div class="item-left">
        <img
          :src="item.icon"
          :alt="item.title"
          class="item-icon"
        >
        <span class="item-title">{{item.title}}</span>
      </div>
      <img
        src="@/static/右箭头.png"
        alt="箭头"
        class="arrow-icon"
      >
    </div>
  </div>
</template>

<script>
export default {
  name: "MyList",
  data() {
    return {
      listItems: [
        {
          type: "history",
          title: "历史预约",
          icon: "../../static/历史预约.png",
        },
        {
          type: "dynamics",
          title: "我的动态",
          icon: "../../static/动态.png",
        },
        {
          type: "courses",
          title: "我的课程",
          icon: "../../static/课程.png",
        },
        {
          type: "about",
          title: "关于KeepFit",
          icon: "../../static/关于.png",
        },
        {
          type: "logout",
          title: "退出登录",
          icon: "../../static/退出.png",
        },
      ],
    };
  },
  methods: {
    handleClick(type) {
      const item = this.listItems.find(item => item.type === type);
      console.log('点击的项目:', item.title);
      
      if (type === 'logout') {
        uni.reLaunch({
          url: "/pages/login/login",
        });
      }
      // 可以根据不同的type添加其他处理逻辑
    },
  },
};
</script>

<style scoped>
.my-list {
  background: #fff;
  border-radius: 12px;
  padding: 0px 0 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  margin: 4px 0;
  border-bottom: 1px solid #f0f0f0;
  transition: all 0.3s ease;
}

.list-item:hover {
  background-color: #f9f9f9;
  transform: translateX(4px);
}

.list-item:last-child {
  border-bottom: none;
  margin-bottom: 0;
}

.item-left {
  display: flex;
  align-items: center;
  gap: 16px;
}

.item-icon {
  width: 24px;
  height: 24px;
}

.item-title {
  font-size: 15px;
  color: #333;
  font-weight: 500;
}

.arrow-icon {
  width: 16px;
  height: 16px;
  opacity: 0.5;
  transition: opacity 0.3s ease;
}

.list-item:hover .arrow-icon {
  opacity: 0.8;
}

.logout {
  margin-top: 8px;
  border-radius: 8px;
  color: #ff4d4f;
}

.logout .item-title {
  color: #ff4d4f;
}

.logout:hover {
  background-color: #fff1f0;
}

</style>