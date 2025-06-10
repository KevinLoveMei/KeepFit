<template>
  <div class="data-forms">
    <div class="data-item" 
         v-for="(item, index) in dataList" 
         :key="index"
         @click="handleClick(item.type)">
      <div class="main-content">
        <div class="left-content">
          <div class="title-row">
            <div class="title">{{item.title}}</div>
            <img class="arrow" src="@/static/右箭头.png" alt="箭头">
          </div>
          <div class="data">
            <span class="number">{{item.value}}</span>
            <span class="unit">{{item.unit}}</span>
          </div>
          <div class="time-desc">{{item.timeDesc}}</div>
        </div>
        <div class="progress-bars">
          <div class="bar-container">
            <div v-for="(bar, idx) in 7" 
                 :key="idx" 
                 class="progress-bar"
                 :class="{ active: idx < item.progress }">
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MyDataThreeForm',
  data() {
    return {
      dataList: [
        {
          type: 'exercise',
          title: '我的数据',
          value: 0,
          unit: '分钟',
          timeDesc: '本周运动',
          progress: 3 // 显示3个柱子点亮
        },
        {
          type: 'weight',
          title: '体重数据',
          value: 0,
          unit: '公斤',
          timeDesc: '近12个月',
          progress: 5
        },
        {
          type: 'total',
          title: '总运动数据',
          value: 0,
          unit: '分钟',
          timeDesc: '近12个月',
          progress: 4
        }
      ]
    }
  },
  methods: {
    handleClick(type) {
      console.log('clicked:', type)
    }
  }
}
</script>

<style scoped>
.data-forms {
  padding: 10px 10px 0px;
}

.data-item {
  background: #fff;
  padding: 15px 20px;
  margin-bottom: 10px;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.title-row {
  display: flex;
  align-items: center;
  width: 100%;
  margin-bottom: 1vw;
}

.left-content {
  width: 100%;
}

.title {
  font-size: 16px;
  color: #333;
  font-weight: 500;
}

.data {
  display: flex;
  align-items: baseline;
  gap: 4px;
}

.number {
  font-size: 24px;
  font-weight: 600;
  color: #2c3e50;
}

.unit {
  font-size: 14px;
  color: #666;
}

.time-desc {
  font-size: 12px;
  color: #999;
}

.arrow {
  width: 16px;
  height: 16px;
  opacity: 0.6;
  margin-left: 8px;
}

.main-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}

.progress-bars {
  margin-left: 20px;
}

.bar-container {
  display: flex;
  gap: 4px;
  align-items: flex-end;
  height: 40px;
}

.progress-bar {
  width: 4px;
  height: 20px;
  background-color: #f0f0f0;
  border-radius: 2px;
}

.progress-bar.active {
  background-color: #4CAF50;
  height: 35px;
}
</style>