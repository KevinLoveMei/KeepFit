<template>
  <div class="reserve-time">
    <div class="coach-info">
      <h2 class="title">排班信息（选择排班时间即可预约教练）</h2>
      <div class="coach-name">
        浙江猛男健身房
        <span class="tag">可预约</span>
      </div>
      <div class="department">私教区</div>
    </div>

    <div class="time-list">
      <transition-group name="list" class="list-container">
        <div
          class="time-item"
          v-for="(item, index) in showList"
          :key="item.date"
        >
          <div class="left-content">
            <div class="date-info">
              <span class="date">{{item.date}}</span>
              <span class="day">{{item.day}}</span>
              <span class="period">{{item.period}}</span>
            </div>
            <div class="appointment-info">
              <div class="type">预约教练</div>
              <div class="price">{{item.price}}元</div>
            </div>
          </div>
          <button
            class="reserve-btn"
            :class="{'success': item.reserved, 'cancel-hover': item.reserved}"
            @click="handleReserve(item)"
          >
            <span class="reserve-text">{{item.reserved ? '预约成功' : '可预约'}}</span>
          </button>
        </div>
      </transition-group>
      <div class="time-item collapse-item">
        <div class="collapse-btn" @click="toggleList">
          {{isCollapsed ? '展开排班' : '收起排班'}}
          <img
            src="../../static/上箭头.png"
            :class="['arrow-icon', {'rotate-icon': isCollapsed}]"
            alt="arrow"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ReserveTime",
  data() {
    return {
      isCollapsed: false,
      timeList: [
        { date: "06.12", day: "星期四", period: "上午", price: 100, reserved: false },
        { date: "06.16", day: "星期一", period: "下午", price: 100, reserved: false },
        { date: "06.19", day: "星期四", period: "上午", price: 100, reserved: false },
        { date: "06.23", day: "星期一", period: "下午", price: 100, reserved: false }
      ]
    }
  },
  computed: {
    showList() {
      return this.isCollapsed ? this.timeList.slice(0, 2) : this.timeList;
    }
  },
  methods: {
    handleReserve(item) {
      item.reserved = !item.reserved;
      console.log(item.reserved ? "预约成功:" : "取消预约:", item);
    },
    toggleList() {
      this.isCollapsed = !this.isCollapsed;
    }
  },
};
</script>

<style scoped>
.reserve-time {
  padding: 15px;
  background: #f5f5f5;
}

.title {
  font-size: 16px;
  color: #333;
  margin-bottom: 10px;
}

.coach-info {
  background: #fff;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 5px;
}

.coach-name {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 8px;
}

.tag {
  font-size: 12px;
  color: #4b8bff;
  border: 1px solid #4b8bff;
  padding: 2px 8px;
  border-radius: 12px;
  margin-left: 10px;
}

.department {
  font-size: 14px;
  color: #666;
}

.time-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #fff;
  padding: 15px;
  position: relative;
}

.left-content {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.date-info {
  display: flex;
  align-items: center;
}

.day,
.period {
  color: #666;
  margin-left: 8px;
}

.appointment-info {
  display: flex;
  gap: 10px;
  color: #666;
  font-size: 14px;
}

.price {
  color: #ff4d4f;
}

.reserve-btn {
  position: absolute;
  right: 15px;
  background: #4b8bff;
  color: #fff;
  border: none;
  border-radius: 15px;
  font-size: 12px;
  min-width: 60px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.reserve-btn.success {
  background: #52c41a;
}

.reserve-btn.cancel-hover:hover {
  background: #ff4d4f;
}

.reserve-btn.cancel-hover:hover .reserve-text {
  content: '取消预约';
}

.reserve-text {
  padding: 0 10px;
}

.collapse-item {
  justify-content: center;
}

.collapse-btn {
  background: #fff;
  color: #666;
  border: 1px solid #ddd;
  font-size: 14px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  border-radius: 20px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
  padding: 8px 0;
  transition: all 0.3s ease;
}

.collapse-btn:hover {
  background: #f5f5f5;
}

.arrow-icon {
  width: 16px;
  height: 16px;
  object-fit: contain;
  transition: all 0.3s ease;
}

.rotate-icon {
  transform: rotate(180deg);
}

.time-list {
  overflow: hidden;
}

.list-container {
  display: block;
  transition: height 0.3s ease-in-out;
}

/* 修改列表项动画 */
.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease-in-out;
  position: relative;
}

.list-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}

.list-leave-to {
  opacity: 0;
  transform: translateY(30px);
  position: absolute;
  width: 100%;
}

.list-move {
  transition: transform 0.3s ease-in-out;
}
</style>