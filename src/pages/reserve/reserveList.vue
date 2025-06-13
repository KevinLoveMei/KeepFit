<template>
  <div class="coach-list">

    <div
      class="mask"
      v-show="showMask"
      @click="handleMaskClick"
    ></div>

    <view class="sort-header">
      <text class="sort-title">教练列表</text>
      <view
        class="sort-button"
        @click="toggleSort"
      >
        <text>{{currentSort.text}}</text>
        <image
          src="../../static/下箭头.png"
          class="sort-icon"
          :class="{'sort-icon-rotate': isSort}"
        ></image>
      </view>
    </view>

    <view
      class="sort-dropdown"
      v-show="showMask"
    >
      <view
        class="sort-item"
        v-for="(item, index) in sortOptions"
        :key="index"
        :class="{'sort-item-active': currentSort.value === item.value}"
        @click="selectSort(item)"
      >
        {{item.text}}
      </view>
    </view>

    <view class="coach-container">
      <view
        class="coach-item"
        v-for="(item, index) in coachList"
        :key="index"
        @click="goToReserveTime(item)"
      >
        <image
          :src="item.image"
          class="coach-image"
        ></image>
        <view class="coach-info">
          <view class="coach-header">
            <text class="coach-name">{{item.name}}</text>
            <text
              class="coach-level"
              :class="'level-' + getStarLevel(item.level)"
            >
              {{item.level}}
            </text>
          </view>
          <view class="coach-tags">
            <text
              class="tag"
              v-for="(tag, idx) in item.tags"
              :key="idx"
            >{{tag}}</text>
          </view>
          <text class="coach-desc">{{item.description}}</text>
        </view>
      </view>
    </view>
  </div>
</template>

<script>
export default {
  name: "ReserveList",
  data() {
    return {
      coachList: [],
      showMask: false,
      isSort: false,
      sortOptions: [
        { text: "默认排序", value: "default" },
        { text: "按照星级排序", value: "star" },
        { text: "按照关注数量排序", value: "popular" },
      ],
      currentSort: { text: "默认排序", value: "default" },
    };
  },
  created() {
    this.fetchCoachList();
  },
  methods: {
    async fetchCoachList() {
      try {
        uni.showLoading({ title: "加载中..." });

        // 兼容微信小程序的请求方式
        const res = await new Promise((resolve, reject) => {
          uni.request({
            url: "http://127.0.0.1:5000/api/coaches",
            method: "GET",
            success: (res) => {
              if (res.statusCode === 200) {
                resolve(res);
              } else {
                reject(new Error(`HTTP状态码: ${res.statusCode}`));
              }
            },
            fail: (err) => reject(err),
          });
        });

        // 处理数据
        this.coachList = res.data.map((item) => ({
          id: item.cid,
          image: item.coachPictureUrl,
          name: item.coachName,
          level: item.coachStar,
          tags: item.coachTag.split(","),
          description: item.coachBrief,
          starLevel: this.getStarLevel(item.coachStar),
          popularity: item.publicity,
          reviews: item.comment
        }));

        // 初始排序
        this.sortCoaches();
      } catch (error) {
        console.error("获取教练列表失败:", error);
        uni.showToast({
          title: error.message || "加载失败",
          icon: "none",
          duration: 2000,
        });
      } finally {
        uni.hideLoading();
      }
    },

    // 将"三星教练"等转换为数字等级
    getStarLevel(starText) {
      const starMap = {
        一星教练: 1,
        二星教练: 2,
        三星教练: 3,
      };
      return starMap[starText] || 0;
    },

    toggleSort() {
      this.showMask = !this.showMask;
      this.isSort = !this.isSort;
    },

    handleMaskClick() {
      this.showMask = false;
      this.isSort = false;
    },

    selectSort(item) {
      this.currentSort = item;
      this.handleMaskClick();
      this.sortCoaches();
    },

    // 排序逻辑
    sortCoaches() {
      if (this.currentSort.value === "default") {
        // 默认排序（按原始顺序）
        this.coachList.sort((a, b) => a.id - b.id);
      } else if (this.currentSort.value === "star") {
        // 按星级排序（降序）
        this.coachList.sort((a, b) => b.starLevel - a.starLevel);
      } else if (this.currentSort.value === "popular") {
        // 按关注数量排序（降序）
        this.coachList.sort((a, b) => b.popularity - a.popularity);
      }
    },

    goToReserveTime(coach) {
      uni.navigateTo({
        url: `/pages/reserveTime/reserveTime?coach=${encodeURIComponent(
          JSON.stringify(coach)
        )}`,
      });
    },
  },
};
</script>

<style scoped>
/* 基础样式 */
.coach-list {
  padding: 10px 15px;
  background: #f5f5f5;
  position: relative;
  min-height: 100vh;
}

/* 头部样式 */
.sort-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 15px;
  background: #fff;
  border-radius: 8px 8px 0 0;
  margin-bottom: 10px;
  position: relative;
  z-index: 100;
}

.sort-title {
  font-size: 16px;
  font-weight: bold;
  color: #333;
}

.sort-button {
  display: flex;
  align-items: center;
  height: 20px;
  line-height: 20px;
  font-size: 14px;
  color: #666;
}

.sort-icon {
  width: 16px;
  height: 16px;
  margin-left: 4px;
  transition: transform 0.3s;
}

.sort-icon-rotate {
  transform: rotate(180deg);
}

/* 遮罩层样式 */
.mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 90; /* 低于头部但高于页面内容 */
}

/* 下拉菜单样式 */
.sort-dropdown {
  position: absolute;
  top: 50px; /* 根据头部高度调整 */
  left: 15px;
  right: 15px;
  background: #fff;
  z-index: 110; /* 高于头部和遮罩 */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.sort-item {
  padding: 12px 15px;
  font-size: 14px;
  color: #333;
  border-bottom: 1px solid #f5f5f5;
}

.sort-item:last-child {
  border-bottom: none;
}

.sort-item-active {
  color: #007aff;
  background-color: #f5f5f5;
}

.sort-item:active {
  background-color: #f0f0f0;
}

/* 教练列表样式 */
.coach-container {
  position: relative;
  z-index: 1;
}

.coach-item {
  display: flex;
  padding: 15px;
  background: #fff;
  border-radius: 8px;
  margin-bottom: 10px;
}

.coach-image {
  width: 80px;
  height: 80px;
  border-radius: 4px;
  margin-right: 12px;
  object-fit: cover;
}

.coach-info {
  flex: 1;
}

.coach-header {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.coach-name {
  font-size: 16px;
  font-weight: bold;
  color: #333;
  margin-right: 10px;
}

/* 教练星级标签样式 */
.coach-level {
  font-size: 12px;
  padding: 2px 6px;
  border-radius: 4px;
  margin-right: 10px;
}

.level-1 {
  color: #007aff;
  background: #e6f2ff;
}

.level-2 {
  color: #ff9500;
  background: #fff7e6;
}

.level-3 {
  color: #ff4d4f;
  background: #fff1f0;
}

.coach-tags {
  display: flex;
  flex-wrap: wrap;
  margin-bottom: 8px;
}

.tag {
  font-size: 12px;
  color: #666;
  background: #f5f5f5;
  padding: 2px 8px;
  border-radius: 4px;
  margin-right: 8px;
  margin-bottom: 4px;
}

.coach-desc {
  font-size: 13px;
  color: #999;
  line-height: 1.4;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>