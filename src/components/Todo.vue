<template>
  <div class="ui segment">
    <h2 class="ui center aligned icon header">
      <i class="list icon"/>Todos
      <div class="sub header">{{tasks.length}} items</div>
    </h2>
    <div class="ui fluid action input">
      <input type="text" placeholder="Enter a new task" v-model="newTask.title" @keyup.enter="add"/>
      <button class="ui icon button" @click="add">
        <i class="plus icon"/>
      </button>
    </div>
    <div class="ui list">
      <Item
        :key="task._id"
        :task="task"
        @del="del"
        v-for="task in tasks"
      />
    </div>
    <!-- <pre>{{tasks}}</pre> -->
  </div>
</template>

<script>
import Item from './todo/Item.vue'

const taskSvc = {
  seq: 0,
  tasks: [],
  findAll () {
    return Promise.resolve(this.tasks.slice(0))
  },
  create (task) {
    task = Object.assign({}, task)
    task._id = ++this.seq + ''
    task.date = new Date()
    this.tasks.push(task)
    return Promise.resolve(task._id)
  },
  remove (_id) {
    for (let i in this.tasks) {
      if (this.tasks[i]._id === _id) {
        this.tasks.splice(i, 1)
        break
      }
    }
    return Promise.resolve()
  }
}

export default {
  components: {Item},
  data () {
    return {
      tasks: [],
      newTask: {
        title: ''
      }
    }
  },
  created () {
    taskSvc.create({title: 'enjoy'})
      .then(this.fetch)
  },
  methods: {
    fetch () {
      return taskSvc.findAll()
        .then(tasks => this.tasks = tasks)
    },
    add () {
      if (!this.newTask) return
      taskSvc.create(this.newTask)
        .then(this.newTask.title = '')
        .then(this.fetch)
    },
    del (_id) {
      taskSvc.remove(_id)
        .then(this.fetch)
    }
  }
}
</script>
