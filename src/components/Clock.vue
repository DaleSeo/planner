<template>
  <div class="ui segment">
    <h2 class="ui center aligned icon header">
      <i class="wait icon"/>
      {{time | formatTime}}
      <div class="sub header">PM</div>
    </h2>
  </div>
</template>

<script>
export default {
  filters: {
    formatTime (time) {
      return moment(time).format('HH:mm')
    }
  },
  data () {
    return {
      time: new Date()
    }
  },
  created () {
    this.updateTime()
  },
  methods: {
    updateTime () {
      this.time = new Date()
      setTimeout(this.updateTime, 1000)
    }
  }
}
</script>

<style scoped>
div.ui.circular.segment {
  height: 11em
}
</style>
