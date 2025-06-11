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
          <div class="level-tag">{{ coachInfo.level }}</div>
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
        <button class="follow-btn">+ 关注</button>
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
      default: () => ({})
    }
  },
  data() {
    return {
      coachInfo: {
        avatar: "",
        name: "",
        level: "",
        tags: "",
        description: "",
        followers: 0,
        reviews: 0,
      }
    };
  },
  watch: {
    coachData: {
      handler(newVal) {
        if (newVal) {
          this.coachInfo = {
            ...this.coachInfo,
            avatar: newVal.image,
            name: newVal.name,
            level: newVal.level,
            tags: newVal.tags || [],
            description: newVal.description || "",
          };
        }
      },
      immediate: true
    }
  }
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
  color: #007AFF;
  background-color: #e6f2ff;
  padding: 2px 8px;
  border-radius: 4px;
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
}

.action-button {
  margin-top: 5px; /* 添加上边距，使按钮位置更协调 */
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