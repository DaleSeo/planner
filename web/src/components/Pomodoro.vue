<template>
  <div class="ui segment">
    <h2 class="ui center aligned icon header">
      <i class="icon" :class="icon"/>
      {{display}}
      <div class="sub header">{{title}}</div>
    </h2>
    <div>
      <button class="ui mini icon button" @click="play">
        <i class="play icon"/>
      </button>
      <button class="ui mini icon button" @click="pause">
        <i class="pause icon"/>
      </button>
    </div>
    <br/>
    <div class="ui right labeled input">
      <input v-model="remaining">
      <div class="ui basic label">seconds</div>
    </div>
    <pre>
      {{ {mode, state, timer, remaining} }}
    </pre>
  </div>
</template>

<script>
const config = {
  workDuration: 25,
  breakDuration: 5
}

const WORK = 0, BREAK = 1
const STOP = 0, PLAYING = 1, PAUSED = 2

function zeroFill (n) {
  return (n < 10 ? '0' : '') + n;
}

export default {
  data () {
    return {
      mode: WORK,
      state: STOP,
      timer: null,
      remaining: 25 * 60
    }
  },
  computed: {
    display () {
      return this.minutes + ':' + this.seconds
    },
    minutes () {
      return zeroFill(Math.floor(this.remaining / 60))
    },
    seconds () {
      return zeroFill(this.remaining % 60)
    },
    title () {
      return this.mode === WORK ? 'WORK' : 'BREAK'
    },
    icon () {
      return this.mode === WORK ? 'lab' : 'coffee'
    }
  },
  created () {
    Notification.requestPermission();
  },
  methods: {
    play () {
      console.log('#play')
      if (this.state === PLAYING) return
      this.state = PLAYING
      this.timer = setInterval(_ => this.tick(), 1000)
    },
    pause () {
      console.log('#pause')
      if (this.state === PAUSED) return
      this.state = PAUSED
      if (this.timer) {
        clearInterval(this.timer)
      }
    },
    tick () {
      if (this.remaining <= 0) {
        console.log('remaining:', )
        this.toggleMode()
        this.notify()
      }
      this.remaining--
    },
    toggleMode () {
      this.mode = this.mode === WORK ? BREAK : WORK
      this.remaining = this.mode === WORK ? 25 * 60 : 5 * 60
    },
    notify () {
      if (this.mode === WORK) {
        new Notification('It\'s time to work!')
      } else {
        new Notification('It\'s time to break!')
      }
    }
  }
}
</script>

<style scoped>
div.ui.circular.segment {
  height: 11em
}
</style>
