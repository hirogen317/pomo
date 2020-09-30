<template>
  <q-page>

    <div v-bind:class="[status, 'banner']">
      <div class="status-box">{{displayStatus[status]}}</div>
      <div class="countdown-box">{{Math.floor(pomoTimer / 60)}}分{{parseInt(pomoTimer % 60)}}秒</div>
    </div>

    <div class="action">
      <q-btn :label="buttonLabel" @click="onPomodoroClicked" />
    </div>

  </q-page>
</template>

<style scoped>
.action {
  display: flex;
  justify-content: center;
}
.banner {
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  justify-items: center;
  font-size: 20px;
  font-weight: 600;
}
.status-box {
  flex: 1;
  padding:10px;
}
.countdown-box {
  flex: 1;
  padding:10px;
}
.stopped {
  color: white;
  background-color: #c53d43;
}
.started {
  color:white;
  background-color: #203744;
}
.rest {
  color:white;
  background-color: #38b48b;
}
</style>
<script>


export default {
  name: 'Pomodoro',
  data () {
    return {
      status: 'stopped',
      displayStatus: {
        stopped: '停止中',
        started: '集中',
        rest: '休憩',
      },
      workingInterval: 25 * 60,
      restInterval: 5 * 60,
      pomoTimer: 0,
      repeatFunction: null,
    }
  },
  computed: {
    buttonLabel: function () {
      if (this.status === 'stopped') {
        return 'Start'
      } else if (this.status === 'started') {
        return 'Stop'
      } else if (this.status === 'rest') {
        return 'Start'
      } else {
        return 'Unknown'
      }
    },
  },
  created() {
    this.pomoTimer = this.workingInterval;
    console.log(this.pomoTimer)
  },
  methods: {
    onPomodoroClicked: function () {
      if (this.status === 'started') {
        this.toggleStop();
      } else if (this.status === 'stopped' || this.status === 'rest' ) {
        this.toggleStart();
      }
    },
    toggleStop: function () {
      this.status = 'stopped';
      this.pomoTimer = this.workingInterval;
      if (this.repeatFunction != null) {
        clearInterval(this.repeatFunction);
      }
    },
    toggleStart: function () {
      this.status = 'started';
      this.pomoTimer = this.workingInterval;
      this.repeatFunction = setInterval(this.tick, 1000);
    },
    toggleRest: function () {
      this.status = 'rest';
      this.pomoTimer = this.restInterval;
      this.repeatFunction = setInterval(this.tick, 1000);
    },
    tick: function () {
      this.pomoTimer --;
      if (this.pomoTimer < 1) {
        clearInterval(this.repeatFunction);
        if (this.status === 'rest') {
          this.toggleStart();
        } else if (this.status === 'started') {
          this.toggleRest();
        }
      }
    }
  }
};
</script>
