<template>
  <div>
    <div class="content">
    <Timer v-for="(timer, i) of timers" :time="timer.time" :key="i"></Timer>
    <AddBtn :action="timerAddAction"></AddBtn>
    </div>
    <div class="modal-back-ground" v-show="showModal">
      <div class="modal">
        <label for="input-time">設定秒数</label>
        <input type="number" name="input-time" v-model="inputTime">
        <br>
        <input type="button" value="追加" @click="add">
        <input type="button" value="閉じる" @click="close">
      </div>
    </div>
  </div>
</template>

<script>
import Timer from "./components/Timer";
import AddBtn from "./components/AddBtn";

export default {
  name: "App",
  components: {
    Timer,
    AddBtn
  },
  data() {
    return {
      timers: [],
      inputTime: null,
      showModal: false
    }
  },
  methods: {
    timerAddAction() {
      this.showModal = true;
    },
    add() {
      const timer = {
        time: this.inputTime * 1000
      }
      this.timers.push(timer)
      this.showModal = false
    },
    close() {
      this.showModal = false
    }
  }
};
</script>

<style>
.modal-back-ground {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: #00000060;
  display: flex;
  justify-content: center; /*左右中央揃え*/
  align-items: center;     /*上下中央揃え*/
}
.modal {
  margin: 0 auto;
  background-color: white;
  width: 300px;
  height: 150px;
  text-align: center;
  border-radius: 15px;
}
.modal * {
  margin: 30px 4px 4px 4px;
}
.content {
  position: absolute;
}
.content * {
  margin: 4px;
}
</style>
