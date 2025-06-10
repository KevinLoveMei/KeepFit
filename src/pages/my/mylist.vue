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
      switch (type) {
        case "logout":
          // 处理退出登录
          this.$emit("logout");
          break;
        default:
          // 处理其他页面跳转
          console.log("navigate to:", type);
      }
    },
  },
};
</script>

<style scoped>
.my-list {
  background: #fff;
  border-radius: 8px;
}

.list-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 20px;
  border-bottom: 1px solid #f5f5f5;
}

.list-item:last-child {
  border-bottom: none;
}

.item-left {
  display: flex;
  align-items: center;
  gap: 12px;
}

.item-icon {
  width: 24px;
  height: 24px;
}

.item-title {
  font-size: 16px;
  color: #333;
}

.arrow-icon {
  width: 16px;
  height: 16px;
  opacity: 0.6;
}

.logout {
  border-radius: 8px;
  color: #ff4d4f;
}

.logout .item-title {
  color: #ff4d4f;
}
</style>