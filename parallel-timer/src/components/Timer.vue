<template>
  <div class="timer-flame">
    <div class="close-button" @click="closeAction">
      <span>×</span>
    </div>
    <div :class="{ 'timer-text': true, completed: remainTime === 0 }">
      {{ getTimeText(remainTime) }}
    </div>
    <div class="timer-btn">
      <input
        type="button"
        value="Start"
        @click="startAction"
        v-if="!runningTimer && remainTime !== 0"
      />
      <input
        type="button"
        value="Stop"
        @click="stopAction"
        v-if="runningTimer"
      />
      <input
        type="button"
        value="Reset"
        @click="resetAction"
        v-if="!runningTimer"
      />
    </div>
  </div>
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
        this.alerm.play();
      }
      if (newVal !== 0 && preVal === 0) {
        this.alerm.stop();
      }
    },
  },
  methods: {
    closeAction() {
      this.$emit("close-action");
    },
    getTimeText(time) {
      const minutes = Math.floor(time / 60000);
      const textMinutes = ("0" + minutes).slice(-2);
      const seconds = Math.floor((time % 60000) / 1000);
      const textSeconds = ("0" + seconds).slice(-2);
      const milliSeconds = time % 1000;
      const textMilliSeconds = ("00" + milliSeconds).slice(-3);
      return `${textMinutes}:${textSeconds}:${textMilliSeconds}`;
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
.timer-flame {
  width: 150px;
  height: 100px;
  padding: 10px;
  border: 4px solid gray;
  border-radius: 15px;
  background-color: #bffcfc;
}
.timer-text {
  text-align: center;
  font-size: 20px;
}
.completed {
  color: red;
}
.timer-btn {
  text-align: center;
}
.timer-btn input {
  width: 70px;
  height: 25px;
}
.close-button {
  cursor: pointer;
  padding-left: 130px;
  position: absolute;
}
</style>
