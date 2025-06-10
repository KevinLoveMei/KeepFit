<template>
    <div>
        <homeHeader></homeHeader>
        <homeFilter></homeFilter>
        <homeVideo></homeVideo>
    </div>
</template>

<script>
import homeHeader from "./homeHeader.vue";
import homeFilter from "./homeFilter.vue";
import homeVideo from "./homeVideo.vue";
export default {
  components: {
    homeHeader,
    homeFilter,
    homeVideo
  },
};
</script>

<style>
</style>