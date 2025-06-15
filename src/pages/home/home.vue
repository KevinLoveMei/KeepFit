<template>
  <view>
    <homeHeader></homeHeader>
    <homeFilter @filter-change="handleFilterChange"></homeFilter>
    <homeVideo
      :videoList="filteredVideos"
      ref="videoComponent"
    ></homeVideo>
  </view>
</template>

<script>
import homeHeader from "./homeHeader.vue";
import homeFilter from "./homeFilter.vue";
import homeVideo from "./homeVideo.vue";

export default {
  components: {
    homeHeader,
    homeFilter,
    homeVideo,
  },
  data() {
    return {
      allVideos: [], // 存储所有视频数据
      filterConditions: {
        types: [],
        level: "",
      },
    };
  },
  computed: {
    // 计算属性，根据筛选条件返回过滤后的视频
    filteredVideos() {
      if (this.allVideos.length === 0) return [];

      return this.allVideos.filter((video) => {
        // 难度筛选
        const levelMatch =
          !this.filterConditions.level ||
          video.hard === this.filterConditions.level;

        // 类型筛选（多选）
        const typeMatch =
          this.filterConditions.types.length === 0 ||
          this.filterConditions.types.some((type) => video.type.includes(type));

        return levelMatch && typeMatch;
      });
    },
  },
  methods: {
    // 处理筛选条件变化
    handleFilterChange(conditions) {
      this.filterConditions = conditions;
    },

    // 获取所有视频数据
    async fetchAllVideos() {
      try {
        uni.showLoading({ title: "加载中..." });

        const res = await new Promise((resolve, reject) => {
          uni.request({
            url: "http://127.0.0.1:5000/api/videos",
            method: "GET",
            success: (res) => resolve(res),
            fail: (err) => reject(err),
          });
        });

        if (res.statusCode !== 200) {
          throw new Error(`HTTP错误: ${res.statusCode}`);
        }

        this.allVideos = res.data;
      } catch (error) {
        console.error("获取视频列表失败:", error);
        uni.showToast({ title: "加载失败", icon: "none" });
      } finally {
        uni.hideLoading();
      }
    },
  },
  created() {
    this.fetchAllVideos();
  },
};
</script>