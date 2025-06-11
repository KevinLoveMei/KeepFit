<template>
  <div class="search-header">
    <div class="search-box">
      <image src="../../static/搜索.png" class="search-icon"></image>
      <input 
        type="text" 
        class="search-input" 
        placeholder="请输入教练名称"
        v-model="searchText"
        @input="handleSearch"
      />
      <span class="search-button">搜索教练</span>
    </div>
    
    <!-- 轮播图部分 -->
    <div class="swiper-container">
      <swiper class="swiper" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="500" :circular="true" indicator-active-color="#fff">
        <swiper-item v-for="(item, index) in banners" :key="index">
          <image :src="item.image" class="swiper-image" mode="aspectFill"></image>
        </swiper-item>
      </swiper>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ReserveHeader',
  data() {
    return {
      searchText: '',
      banners: [
        { image: '../../static/视频1.png' },
        { image: '../../static/视频2.png' },
        { image: '../../static/视频3.png' }
      ]
    }
  },
  methods: {
    handleSearch() {
      // 触发搜索事件
      this.$emit('search', this.searchText)
    }
  }
}
</script>

<style scoped>
.search-header {
  padding: 10px 15px;
  background-color: #fff;
}

.search-box {
  position: relative;
  display: flex;
  align-items: center;
  background-color: #f5f5f5;
  border-radius: 20px;
  padding: 8px 15px;
}

.search-icon {
  width: 20px;
  height: 20px;
  margin-right: 8px;
  opacity: 0.6;
}

.search-input {
  flex: 1;
  border: none;
  background: none;
  font-size: 14px;
  color: #333;
}

.search-input::placeholder {
  color: #999;
}

.search-input:focus {
  outline: none;
}

.search-button {
    color: #007AFF;
}

.swiper-container {
  margin-top: 10px;
  width: 100%;
  height: 150px;
}

.swiper {
  width: 100%;
  height: 100%;
}

.swiper-image {
  width: 100%;
  height: 100%;
  border-radius: 8px;
}

</style>