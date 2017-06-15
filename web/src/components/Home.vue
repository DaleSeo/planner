<template>
  <div class="ui container">
    <h1 class="ui dividing header">
      Dale's Timer
    </h1>
    <div class="ui three column centered grid">
      <div class="center aligned column">
        <Clock/>
      </div>
      <div class="center aligned column">
        <Pomodoro/>
      </div>
      <div class="column">
        <Todo/>
      </div>
    </div>
  </div>
</template>

<script>
import Clock from './Clock.vue'
import Pomodoro from './Pomodoro.vue'
import Todo from './Todo.vue'

export default {
  components: {Clock, Pomodoro, Todo}
}
</script>

<style scoped>
div.ui.container {
  margin-top: 50px;
}
</style>
