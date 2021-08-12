<template>
  <div>
    {{ `${elapsedMinutes}:${elapsedSeconds}.${elapsedMilliSeconds}` }}
    {{ remainTime }}
    <input type="button" value="start" @click="startAction" />
    <input type="button" value="stop" @click="stopAction" />
  </div>
</template>
<script>
export default {
  data() {
    return {
      time: 3000,
      startTime: null,
      elapsedTime: 0, // 経過時間
      timeToAdd: 0,
      timerId: null,
    };
  },
  computed: {
    // 経過時間(分)
    elapsedMinutes() {
      const value = Math.floor(this.elapsedTime / 60000);
      return ("0" + value).slice(-2);
    },
    // 経過時間(秒)
    elapsedSeconds() {
      const value = Math.floor((this.elapsedTime % 60000) / 1000);
      return ("0" + value).slice(-2);
    },
    // 経過時間(ﾐﾘ秒)
    elapsedMilliSeconds() {
      const value = this.elapsedTime % 1000;
      return ("00" + value).slice(-3);
    },
    remainTime() {
      const value = this.time - this.elapsedTime;
      return value > 0 ? value : 0;
    },
  },
  methods: {
    countUp() {
      this.timerId = setTimeout(() => {
        this.elapsedTime = Date.now() - this.startTime + this.timeToAdd;
        if (this.time - this.elapsedTime > 0) {
          this.countUp();
        }
      }, 10);
    },
    startAction() {
      this.startTime = Date.now();
      this.countUp();
    },
    stopAction() {
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
