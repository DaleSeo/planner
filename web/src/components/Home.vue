<template>
  <div class="ui container">
    <h1 class="ui dividing header">
      Dale's Timer
    </h1>
    <div>
      <Clock/>
    </div>
  </div>
</template>

<script>
import Clock from './Clock.vue'

export default {
  components: {Clock}
}
</script>

<style scoped>
div.ui.container {
  margin-top: 50px;
}
</style>
