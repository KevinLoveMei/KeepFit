<template>
  <view class="filter-container">
    <!-- 类型筛选 -->
    <view class="filter-section">
      <view class="filter-title">视频类型</view>
      <scroll-view scroll-x class="filter-scroll">
        <view class="filter-tags">
          <view 
            v-for="(item, index) in typeFilters" 
            :key="index"
            :class="['filter-tag', activeTypes.includes(item) ? 'active' : '']"
            @click="selectType(item)"
          >
            {{ item }}
          </view>
        </view>
      </scroll-view>
    </view>
    
    <!-- 难度筛选 -->
    <view class="filter-section">
      <view class="filter-title">视频难度</view>
      <scroll-view scroll-x class="filter-scroll">
        <view class="filter-tags">
          <view 
            v-for="(item, index) in levelFilters" 
            :key="index"
            :class="['filter-tag', activeLevel === item ? 'active' : '']"
            @click="selectLevel(item)"
          >
            {{ item }}
          </view>
        </view>
      </scroll-view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      typeFilters: ['有氧操', '跳绳', '八段锦', 'HIIT', '舞蹈燃脂','帕梅拉','腰腹减脂塑形','瑜伽','跑步','增肌','冥想','瘦腿'],
      levelFilters: ['K1零基础', 'K2初学', 'K3进阶', 'K4强化', 'K5挑战'],
      activeTypes: [], // 改为数组以支持多选
      activeLevel: ''
    }
  },
  methods: {
    selectType(type) {
      const index = this.activeTypes.indexOf(type)
      if (index === -1) {
        // 如果不存在，则添加
        this.activeTypes.push(type)
      } else {
        // 如果已存在，则移除
        this.activeTypes.splice(index, 1)
      }
      
      this.$emit('filter-change', {
        types: this.activeTypes,
        level: this.activeLevel
      })
    },
    selectLevel(level) {
      // 如果点击的是当前已选中的难度，则取消选中
      if (this.activeLevel === level) {
        this.activeLevel = ''
      } else {
        this.activeLevel = level
      }
      
      this.$emit('filter-change', {
        types: this.activeTypes,
        level: this.activeLevel
      })
    }
  }
}
</script>

<style>
.filter-container {
  padding: 10px;
  background-color: #ffffff;
}

.filter-section {
  margin-bottom: 15px;
}

.filter-section:last-child {
  margin-bottom: 0;
}

.filter-title {
  font-size: 14px;
  color: #333;
  margin-bottom: 10px;
  font-weight: 500;
}

.filter-scroll {
  width: 100%;
  white-space: nowrap;
}

.filter-tags {
  display: inline-flex;
  gap: 10px;
}

.filter-tag {
  position: relative;
  padding: 6px 12px;
  border-radius: 16px;
  font-size: 13px;
  color: #666;
  background-color: #f5f5f5;
  border: 1px solid transparent;
  transition: all 0.3s ease;
  cursor: pointer;
  user-select: none;
  white-space: nowrap;
}

.filter-tag.active {
  background-color: #e6f7ff;
  color: #1890ff;
  border-color: #1890ff;
}

.filter-tag.active::after {
  content: '';
  position: absolute;
  right: 4px;
  top: 4px;
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background-color: #1890ff;
}

.filter-tag:active {
  opacity: 0.8;
}
</style>