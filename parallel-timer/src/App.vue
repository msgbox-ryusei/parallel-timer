<template>
  <div>
    <div class="content">
      <input type="button" value="all start" @click="allStart" />
      <input type="button" value="all stop" @click="allStop" />
      <Timer
        v-for="(timer, i) of timers"
        :time="timer.time"
        :key="i"
        :ref="`timer${i}`"
        v-on:close-action="() => openCloseModal(i)"
      ></Timer>
      <AddBtn :action="timerAddAction"></AddBtn>
    </div>
    <div class="modal-back-ground" v-show="showAddModal">
      <div class="modal">
        <label for="input-time">設定秒数</label>
        <input type="number" name="input-time" v-model="inputTime" />
        <br />
        <input type="button" value="追加" @click="add" />
        <input type="button" value="閉じる" @click="close" />
      </div>
    </div>
    <div class="modal-back-ground" v-show="showCloseModal">
      <div class="modal">
        <h5>このタイマーを閉じてもよろしいでしょうか。</h5>
        <br />
        <input type="button" value="はい" @click="closeTimer" />
        <input type="button" value="いいえ" @click="close" />
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
    AddBtn,
  },
  data() {
    return {
      timers: [],
      inputTime: 0,
      showAddModal: false,
      showCloseModal: false,
      closeIndex: 0,
    };
  },
  methods: {
    timerAddAction() {
      this.showAddModal = true;
    },
    add() {
      if (!this.inputTime) {
        return;
      }
      const timer = {
        time: this.inputTime * 1000,
      };
      this.timers.push(timer);
      this.showAddModal = false;
    },
    close() {
      this.showAddModal = false;
      this.showCloseModal = false;
    },
    allStart() {
      let startActions = [];
      this.timers.forEach((timer, i) => {
        startActions.push(this.$refs[`timer${i}`].startAction);
      });
      Promise.all(startActions.map((v) => v()));
    },
    allStop() {
      this.timers.forEach((timer, i) => {
        this.$refs[`timer${i}`].stopAction();
      });
    },
    openCloseModal(i) {
      this.closeIndex = i;
      this.showCloseModal = true;
    },
    closeTimer() {
      this.timers.splice(this.closeIndex, 1);
      this.showCloseModal = false;
    },
  },
};
</script>

<style>
.modal-back-ground {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #00000060;
  display: flex;
  justify-content: center; /*左右中央揃え*/
  align-items: center; /*上下中央揃え*/
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
