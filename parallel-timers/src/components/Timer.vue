<template>
  <v-card max-width="250">
    <div class="close-button">
      <span class="ma-1 cursor-button" @click="closeAction">×</span>
    </div>
    <v-card-text class="align-center">
      <div
        :class="{
          'timer-text': true,
          'mb-3': true,
          completed: remainTime === 0,
        }"
      >
        {{ getTimeText(remainTime) }}
      </div>
      <div class="timer-btn">
        <v-btn @click="startAction" v-if="!runningTimer && remainTime !== 0" class="mx-1">
          Start
        </v-btn>
        <v-btn @click="stopAction" v-if="runningTimer" class="mx-1"> Stop </v-btn>
        <v-btn @click="resetAction" v-if="!runningTimer" class="mx-1"> Reset </v-btn>
      </div>
    </v-card-text>
  </v-card>
</template>
<script>
export default {
  props: {
    time: {
      required: true,
      type: Number,
    },
  },
  data() {
    return {
      startTime: null,
      elapsedTime: 0, // 経過時間
      timeToAdd: 0,
      timerId: null,
      runningTimer: false,
      alerm: new Audio("./alerm.mp3"),
    };
  },
  computed: {
    remainTime() {
      const value = this.time - this.elapsedTime;
      return value > 0 ? value : 0;
    },
  },
  watch: {
    remainTime(newVal, preVal) {
      if (newVal !== preVal && newVal === 0) {
        // this.alerm.play();
      }
      if (newVal !== 0 && preVal === 0) {
        // this.alerm.stop();
      }
    },
  },
  methods: {
    closeAction() {
      this.$emit("close-action");
    },
    getTimeText(time) {
      const secondsTime = time / 1000;
      const hours = Math.floor(secondsTime / 3600);
      const minutes = Math.floor((secondsTime % 3600) / 60);
      const textMinutes = ("0" + minutes).slice(-2);
      const seconds = Math.floor(secondsTime % 60);
      const textSeconds = ("0" + seconds).slice(-2);
      // const milliSeconds = time % 1000;
      // const textMilliSeconds = ("00" + milliSeconds).slice(-3);
      return `${hours}:${textMinutes}:${textSeconds}`;
    },
    countUp() {
      this.timerId = setTimeout(() => {
        this.elapsedTime = Date.now() - this.startTime + this.timeToAdd;
        if (this.time - this.elapsedTime > 0) {
          this.countUp();
        } else {
          this.stopAction();
        }
      }, 10);
    },
    startAction() {
      if (!this.runningTimer) {
        this.runningTimer = true;
        this.startTime = Date.now();
        this.countUp();
      }
    },
    stopAction() {
      if (this.runningTimer) {
        this.runningTimer = false;
        clearTimeout(this.timerId);
        this.timeToAdd += Date.now() - this.startTime;
      }
    },
    resetAction() {
      this.elapsedTime = 0;
      this.timeToAdd = 0;
    },
  },
};
</script>
<style>
.timer-text {
  text-align: center;
  font-size: 25px;
}
.completed {
  color: red;
}
.align-center {
  text-align: center;
}
.close-button {
  text-align: right;
  font-size: 18px;
}
.cursor-button {
  cursor: pointer;
}
</style>
