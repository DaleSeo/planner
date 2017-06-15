<template>
  <div class="ui message">
    <i class="close icon" @click="del"/>
    <div class="header">
      {{task.title}}
    </div>
    <i class="wait icon"/>
      {{taskDate}}
  </div>
</template>

<script>
export default {
  props: ['task'],
  computed: {
    taskDate () {
      return moment(this.task.date).format('HH:mm:ss')
    }
  },
  methods: {
    del () {
      this.$emit('del', this.task._id)
    }
  }
}
</script>
