<template>
  <div class="ui form message">
    <div class="field">
      <label>Work: <span>{{settings.workDuration}}</span></label>
      <input type="range" min="0" max="60" v-model="settings.workDuration" style="width: 100%">
    </div>
    <div class="field">
      <label>Break: <span>{{settings.breakDuration}}</span></label>
      <input type="range" min="0" max="60" v-model="settings.breakDuration" style="width: 100%">
    </div>
    <pre>{{settings}}</pre>
  </div>
</template>

<script>
export default {
  props: ['settings']
}
</script>
