<template>
  <div class="ui container">
    <h1 class="ui dividing header">
      Planner
    </h1>
    <div class="ui three column centered grid">
      <div class="center aligned column">
        <Clock/>
      </div>
      <div class="center aligned column">
        <Timer/>
      </div>
      <div class="column">
        <Todo/>
      </div>
    </div>
  </div>
</template>

<script>
import Clock from './Clock.vue'
import Timer from './Timer.vue'
import Todo from './Todo.vue'

export default {
  components: {Clock, Timer, Todo}
}
</script>

<style scoped>
div.ui.container {
  margin-top: 50px;
}
</style>
