<template>
  <div class="coach-profile">
    <div class="coach-info">
      <div class="avatar-container">
        <img
          class="avatar"
          :src="coachInfo.avatar"
          alt="教练头像"
        >
      </div>
      <div class="basic-info">
        <div class="name-level">
          <h2 class="name">{{ coachInfo.name }}</h2>
          <div
            class="level-tag"
            :class="'level-' + getStarLevel(coachInfo.level)"
          >
            {{ coachInfo.level }}
          </div>
        </div>
        <div class="department">
          <span
            class="tag"
            v-for="(tag, index) in coachInfo.tags"
            :key="index"
          >
            {{ tag }}
          </span>
        </div>
        <div class="qualifications">
          {{ coachInfo.description || "暂无介绍" }}
        </div>
      </div>
      <div class="action-button">
        <button
          class="follow-btn"
          :class="{ 'followed': isFollowing }"
          @click="toggleFollow"
        >
          {{ isFollowing ? '已关注' : '+ 关注' }}
        </button>
      </div>
    </div>

    <div class="function-tabs">
      <div class="tab-item">
        <i class="star-icon"></i>
        <span>关注 {{ coachInfo.followers }}</span>
      </div>
      <div class="tab-item">
        <i class="comment-icon"></i>
        <span>评价 {{ coachInfo.reviews }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "coachProfile",
  props: {
    coachData: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      coachInfo: {
        id: "",
        avatar: "",
        name: "",
        level: "",
        tags: "",
        description: "",
        followers: 0,
        reviews: 0,
      },
      isFollowing: false,
    };
  },
  methods: {
    // 添加星级转换方法
    getStarLevel(starText) {
      const starMap = {
        一星教练: 1,
        二星教练: 2,
        三星教练: 3,
      };
      return starMap[starText] || 0;
    },

    // 添加获取关注状态的方法
    getFollowStatus() {
      const followedCoaches = uni.getStorageSync('followedCoaches') || [];
      this.isFollowing = followedCoaches.includes(this.coachInfo.id);
    },

    // 修改切换关注状态方法
    async toggleFollow() {
      const url = `http://127.0.0.1:5000/api/coaches/${this.coachInfo.id}/${
        this.isFollowing ? "decrease_publicity" : "increase_publicity"
      }`;

      wx.request({
        url: url,
        method: "PATCH",
        success: () => {
          this.coachInfo.followers += this.isFollowing ? -1 : 1;
          this.isFollowing = !this.isFollowing;
          
          // 更新本地存储中的关注状态
          let followedCoaches = uni.getStorageSync('followedCoaches') || [];
          if (this.isFollowing) {
            followedCoaches.push(this.coachInfo.id);
          } else {
            followedCoaches = followedCoaches.filter(id => id !== this.coachInfo.id);
          }
          uni.setStorageSync('followedCoaches', followedCoaches);
        },
        fail: (error) => {
          console.error("操作失败:", error);
          wx.showToast({
            title: "操作失败",
            icon: "none",
          });
        },
      });
    },
  },
  watch: {
    coachData: {
      handler(newVal) {
        if (newVal) {
          this.coachInfo = {
            ...this.coachInfo,
            id: newVal.id || "",
            avatar: newVal.image,
            name: newVal.name,
            level: newVal.level,
            tags: newVal.tags || [],
            description: newVal.description || "",
            followers: newVal.popularity || 0,
            reviews: newVal.reviews || 0,
          };
          // 在教练信息更新后获取关注状态
          this.getFollowStatus();
        }
      },
      immediate: true,
    },
  },
  // 添加页面显示时的钩子
  onShow() {
    // 每次页面显示时重新获取关注状态
    if (this.coachInfo.id) {
      this.getFollowStatus();
    }
  },
};
</script>

<style scoped>
.coach-profile {
  padding: 20px;
  background: #fff;
}

.coach-info {
  display: flex;
  align-items: flex-start;
  margin-bottom: 20px;
}

.avatar-container {
  margin-right: 15px;
}

.avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  border: 1px solid #eee;
}

.basic-info {
  flex: 1;
}

.name-level {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.name {
  font-size: 18px;
  font-weight: bold;
  margin: 0;
  margin-right: 10px;
}

.level-tag {
  font-size: 12px;
  padding: 2px 8px;
  border-radius: 4px;
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

.department {
  font-size: 14px;
  color: #666;
  margin-bottom: 5px;
}

.qualifications {
  font-size: 12px;
  color: #999;
  line-height: 1.4;
}

.follow-btn {
  background: transparent;
  color: white;
  background-color: #2196f3;
  border: 1px solid #2196f3;
  border-radius: 20px;
  font-size: 13px;
  font-weight: normal;
  cursor: pointer;
  transition: all 0.3s;
}

.follow-btn.followed {
  background-color: #f5f5f5;
  color: #666;
  border-color: #ddd;
}

.action-button {
  margin-top: 5px;
}

.function-tabs {
  display: flex;
  justify-content: space-around;
  border-top: 1px solid #eee;
  padding-top: 15px;
}

.tab-item {
  text-align: center;
  color: #666;
  font-size: 14px;
}

.tab-item i {
  display: block;
  font-size: 20px;
  margin-bottom: 5px;
}

.tag {
  font-size: 12px;
  color: #666;
  background: #f5f5f5;
  padding: 2px 8px;
  border-radius: 12px;
  margin-right: 8px;
  margin-bottom: 4px;
  display: inline-block;
}
</style>