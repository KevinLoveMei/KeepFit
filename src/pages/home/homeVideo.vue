<template>
  <view class="video-container">
    <view class="video-grid">
      <view
        v-for="(item, index) in videoList"
        :key="index"
        class="video-card"
        @click="handleVideoClick(item)"
      >
        <view class="video-cover">
          <image
            :src="item.coverUrl"
            mode="aspectFill"
            class="cover-image"
          />
          <view class="video-duration">{{ item.duration }}</view>
        </view>
        <view class="video-info">
          <text class="video-title">{{ item.title }}</text>
          <view class="video-stats">
            <text class="video-views">{{ item.views }}次观看</text>
            <text class="video-author">{{ item.author }}</text>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      videoList: [
        {
          id: 1,
          title: "10分钟全身燃脂运动",
          coverUrl: "/static/视频1.png",
          duration: "10:00",
          views: "2.5万",
          author: "健身教练小明",
        },
        {
          id: 2,
          title: "10分钟全身燃脂运动",
          coverUrl: "/static/视频2.png",
          duration: "10:00",
          views: "2.5万",
          author: "健身教练小明",
        },
        {
          id: 3,
          title: "10分钟全身燃脂运动",
          coverUrl: "/static/视频3.png",
          duration: "10:00",
          views: "2.5万",
          author: "健身教练小明",
        },
        {
          id: 4,
          title: "10分钟全身燃脂运动",
          coverUrl: "/static/视频4.png",
          duration: "10:00",
          views: "2.5万",
          author: "健身教练小明",
        },
        {
          id: 5,
          title: "10分钟全身燃脂运动",
          coverUrl: "/static/视频5.png",
          duration: "10:00",
          views: "2.5万",
          author: "健身教练小明",
        },
        {
          id: 6,
          title: "10分钟全身燃脂运动",
          coverUrl: "/static/视频6.png",
          duration: "10:00",
          views: "2.5万",
          author: "健身教练小明",
        },
      ],
    };
  },
  methods: {
    handleVideoClick(video) {
      // 处理视频点击事件
      console.log("点击视频:", video.title);
    },
  },
};
</script>

<style>
.video-container {
  padding: 10px;
}

.video-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
  padding: 5px;
}

.video-card {
  background: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

.video-cover {
  position: relative;
  width: 100%;
  padding-top: 56.25%; /* 16:9 宽高比 */
}

.cover-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.video-duration {
  position: absolute;
  bottom: 8px;
  right: 8px;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 2px 4px;
  border-radius: 4px;
  font-size: 12px;
}

.video-info {
  padding: 8px;
}

.video-title {
  font-size: 14px;
  font-weight: 500;
  color: #333;
  line-height: 1.3;
  margin-bottom: 4px;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.video-stats {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 12px;
  color: #666;
}

.video-views {
  color: #999;
}

.video-author {
  color: #666;
  font-weight: 500;
}
</style>