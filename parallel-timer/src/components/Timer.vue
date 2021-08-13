<template>
<v-card>
  <div>
    {{ getTimeText(remainTime) }}
    <v-btn>test</v-btn>
    <input type="button" value="start" @click="startAction" v-if="!runningTimer && remainTime !== 0"/>
    <input type="button" value="stop" @click="stopAction" v-if="runningTimer"/>
    <input type="button" value="reset" @click="resetAction" v-if="remainTime === 0">
  </div>
</v-card>
</template>
<script>
export default {
  props: {
    time: {
      required: true,
      type: Number
    }
  },
  data() {
    return {
      startTime: null,
      elapsedTime: 0, // 経過時間
      timeToAdd: 0,
      timerId: null,
      runningTimer: false
    };
  },
  computed: {
    remainTime() {
      const value = this.time - this.elapsedTime;
      return value > 0 ? value : 0;
    },
  },
  methods: {
    getTimeText(time) {
      const minutes = Math.floor(time / 60000);
      const textMinutes = ("0" + minutes).slice(-2);
      const seconds = Math.floor((time % 60000) / 1000);
      const textSeconds = ("0" + seconds).slice(-2);
      const milliSeconds = time % 1000;
      const textMilliSeconds = ("00" + milliSeconds).slice(-3);
      return `${textMinutes}:${textSeconds}:${textMilliSeconds}`
    },
    countUp() {
      this.timerId = setTimeout(() => {
        this.elapsedTime = Date.now() - this.startTime + this.timeToAdd;
        if (this.time - this.elapsedTime > 0) {
          this.countUp();
        }else {
          this.stopAction();
        }
      }, 10);
    },
    startAction() {
      this.runningTimer = true;
      this.startTime = Date.now();
      this.countUp();
    },
    stopAction() {
      this.runningTimer = false;
      clearTimeout(this.timerId);
      this.timeToAdd += Date.now() - this.startTime;
    },
    resetAction() {
      this.elapsedTime = 0;
      this.timeToAdd = 0;
    },
  },
};
</script>
