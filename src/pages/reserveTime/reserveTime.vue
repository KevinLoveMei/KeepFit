<template>
  <div>
    <reserveTimeHeader :coachData="coachData"></reserveTimeHeader>
    <reserveTimeMain></reserveTimeMain>
  </div>
</template>

<script>
import reserveTimeHeader from "./reserveTimeHeader.vue";
import reserveTimeMain from "./reserveTimeMain.vue";
export default {
  components: {
    reserveTimeHeader,
    reserveTimeMain
  },
  data() {
    return {
      coachData: null
    }
  },
  onLoad(option) {
    if (option.coach) {
      this.coachData = JSON.parse(decodeURIComponent(option.coach));
    }
  }
};
</script>

<style>
</style>