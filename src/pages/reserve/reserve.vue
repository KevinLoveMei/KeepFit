<template>
  <div>
    <reserveHeader></reserveHeader>
    <reserveList></reserveList>
  </div>
</template>

<script>
import reserveHeader from "./reserveHeader.vue";
import reserveList from "./reserveList.vue";
export default {
  components: {
    reserveHeader,
    reserveList,
  },
};
</script>

<style>
</style>