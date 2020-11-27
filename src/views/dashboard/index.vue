<template>
  <div class="dashboard-container">
    <div class="dashboard-text">
      <!-- <img src="./sitefiles/9CA903139164501C6C9FDA20E6625773.png" style="height:100vh"/> -->
      <china-map></china-map>
    </div>
  </div>
</template>

<script>
import ChinaMap from './components/ChinaMap'

export default {
  name: 'Dashboard',
  components: {
    ChinaMap,
  }
}
</script>

<style lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
