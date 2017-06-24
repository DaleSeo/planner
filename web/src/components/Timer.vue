<template>
  <div class="ui segment">
    <h2 class="ui center aligned icon header">
      <i class="icon" :class="icon"/>
      {{minutes}}:{{seconds}}
      <div class="sub header">{{title}}</div>
    </h2>
    <div>
      <button class="ui mini icon button" @click="play">
        <i class="play icon"/>
      </button>
      <button class="ui mini icon button" @click="pause">
        <i class="pause icon"/>
      </button>
      <button class="ui mini icon button" @click="backward">
        <i class="step backward icon"/>
      </button>
      <button class="ui mini icon button" @click="forward">
        <i class="step forward icon"/>
      </button>
      <button class="ui mini icon button" @click="plus">
        <i class="step plus icon"/>
      </button>
      <button class="ui mini icon button" @click="minus">
        <i class="step minus icon"/>
      </button>
      <button class="ui mini icon button" @click="toggleSettings">
        <i class="setting icon"/>
      </button>
    </div>
    <Settings :settings="settings" v-if="showSettings"/>
    <!-- <pre>
      {{ {mode, state, timer, remaining} }}
    </pre> -->
  </div>
</template>

<script>
import Settings from './timer/Settings.vue'

const WORK_MODE = {
  duration: 25,
  title: 'Work',
  icon: 'student'
}

const BREAK_MODE = {
  duration: 5,
  title: 'Break',
  icon: 'coffee'
}

const STOP = 0, PLAYING = 1, PAUSED = 2

function zeroFill (n) {
  return (n < 10 ? '0' : '') + n;
}

export default {
  components: {Settings},
  data () {
    return {
      mode: WORK_MODE,
      state: STOP,
      timer: null,
      remaining: 0,
      settings: {
        workDuration: 25,
        breakDuration: 5
      },
      showSettings: false
    }
  },
  computed: {
    minutes: {
      get () {
        return zeroFill(Math.floor(this.remaining / 60))
      },
      set (val) {
        console.log('#val:' + val)
        console.log('#set:', parseInt(val) * 60 + this.seconds)
        this.remaining = parseInt(val) * 60 + parseInt(this.seconds)
      }
    },
    seconds: {
      get () {
        return zeroFill(this.remaining % 60)
      },
      set (val) {
        this.remaining = parseInt(this.minutes) * 60 + parseInt(val)
      }
    },
    title () {
      return this.mode.title
    },
    icon () {
      return this.mode.icon
    }
  },
  watch: {
    'settings.workDuration': function (val) {
      WORK_MODE.duration = val
    },
    'settings.breakDuration': function (val) {
      BREAK_MODE.duration = val
    }
  },
  created () {
    Notification.requestPermission()
  },
  methods: {
    play () {
      console.log('#play')
      if (this.state === PLAYING) return
      if (this.state === STOP) {
        this.remaining = this.mode.duration * 60
      }
      this.state = PLAYING
      this.timer = setInterval(_ => this.tick(), 1000)
    },
    pause () {
      console.log('#pause')
      if (this.state === PAUSED || this.state === STOP) return
      this.state = PAUSED
      if (this.timer) {
        clearInterval(this.timer)
      }
    },
    backward () {
      console.log('#backward')
      this.remaining = this.mode.duration * 60
    },
    forward () {
      console.log('#forward')
      this.remaining = 0
    },
    plus () {
      this.minutes++
    },
    minus () {
      this.minutes--
    },
    toggleSettings () {
      this.showSettings = !this.showSettings
    },
    tick () {
      if (this.remaining <= 0) {
        this.toggleMode()
        this.notify()
      }
      this.remaining--
    },
    toggleMode () {
      this.mode = this.mode === WORK_MODE ? BREAK_MODE : WORK_MODE
      this.remaining = this.mode.duration * 60
    },
    notify () {
      if (this.mode === WORK_MODE) {
        new Notification('It\'s time to work!')
      } else {
        new Notification('It\'s time to break!')
      }
    }
  }
}
</script>
