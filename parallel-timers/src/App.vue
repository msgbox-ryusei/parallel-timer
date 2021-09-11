<template>
  <v-app>
    <v-main class="back-ground">
      <div class="side-menu">
        <ul class="menu">
          <li @click="allStart"><a>All Start</a></li>
          <v-divider></v-divider>
          <li @click="allStop"><a>All Stop</a></li>
          <v-divider></v-divider>
          <li @click="allReset"><a>All Reset</a></li>
        </ul>
      </div>
      <div class="contant" id="top">
        <v-dialog v-model="addDialog" width="500" color>
          <template v-slot:activator="{ on, attrs }">
            <v-row>
              <v-col>
                <v-btn
                  color="green lighten-1"
                  dark
                  v-bind="attrs"
                  v-on="on"
                  class="mx-3 mt-2"
                >
                  Add Timer
                </v-btn>
              </v-col>
              <v-spacer></v-spacer>
              <v-col>
                <v-switch
                  v-model="playSound"
                  label="Sound♪"
                  color="info"
                  hide-details
                ></v-switch
              ></v-col>
            </v-row>
          </template>

          <v-card>
            <v-card-title class="text-h5 orange lighten-2"> Add Timer </v-card-title>
            <v-card-text>
              <v-container>
                <v-row>
                  <v-col cols="8">
                    <v-text-field
                      type="text"
                      label="タイトル"
                      v-model="inputTitle"
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-row>
                  <v-col cols="4"
                    ><InputNumber v-model="inputHour" label="時間" :max="99"></InputNumber
                  ></v-col>
                  <v-col cols="4"
                    ><InputNumber
                      v-model="inputMinutes"
                      label="分"
                      :max="59"
                    ></InputNumber
                  ></v-col>
                  <v-col cols="4"
                    ><InputNumber v-model="inputSecond" label="秒" :max="59"></InputNumber
                  ></v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" text @click="addTimer"> OK </v-btn>
              <v-btn color="primary" text @click="addDialog = false"> Cancel </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="deleteDialog" width="500">
          <v-card>
            <v-card-title> </v-card-title>
            <v-card-text> タイマーを削除してもよろしいですか。 </v-card-text>

            <v-divider></v-divider>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="primary" text @click="deleteTimer"> OK </v-btn>
              <v-btn color="primary" text @click="deleteDialog = false"> Cancel </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <div class="d-flex flex-wrap">
          <Timer
            v-for="timer of timers"
            :key="timer.id"
            class="ma-3"
            :time="timer.time"
            :title="timer.title"
            ref="timers"
            :sound="playSound"
            @close-action="openDeleteDialog(timer.id)"
          ></Timer>
        </div>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import Timer from "./components/Timer";
import InputNumber from "./components/InputNumber";

export default {
  name: "App",

  components: {
    Timer,
    InputNumber,
  },

  data: () => ({
    timerIdSequence: 1,
    timers: [],
    inputHour: "",
    inputMinutes: "",
    inputSecond: "",
    inputTitle: "",
    addDialog: false,
    deleteDialog: false,
    deleteId: 0,
    playSound: false,
  }),
  methods: {
    addTimer() {
      let sum = 0;
      if (this.inputHour) {
        sum = sum + this.inputHour * 3600 * 1000;
      }
      if (this.inputMinutes) {
        sum = sum + this.inputMinutes * 60 * 1000;
      }
      if (this.inputSecond) {
        sum = sum + this.inputSecond * 1000;
      }
      if (sum !== 0) {
        const timer = {
          time: sum,
          id: this.timerIdSequence,
          title: this.inputTitle,
        };
        this.timers.unshift(timer);
        this.timerIdSequence++;
        this.addDialog = false;
      }
    },
    openDeleteDialog(timerId) {
      this.deleteId = timerId;
      this.deleteDialog = true;
    },
    deleteTimer() {
      const newTimers = this.timers.filter((v) => v.id !== this.deleteId);
      this.timers = newTimers;
      this.deleteDialog = false;
      this.deleteId = 0;
    },
    allStart() {
      const startActions = this.$refs.timers.map((ref) => ref.startAction);
      Promise.all(startActions.map((v) => v()));
    },
    allStop() {
      const stopActions = this.$refs.timers.map((ref) => ref.stopAction);
      Promise.all(stopActions.map((v) => v()));
    },
    allReset() {
      this.allStop();
      const resetActions = this.$refs.timers.map((ref) => ref.resetAction);
      Promise.all(resetActions.map((v) => v()));
    },
  },
};
</script>
<style>
.back-ground {
  background-color: #fff9c4;
}
.content {
  margin: 0 auto;
}
.side-menu {
  position: fixed;
  top: 10vh;
  right: 0;
  z-index: 100000;
  font-size: 0;
}

.side-menu li a {
  -webkit-writing-mode: vertical-rl;
  -ms-writing-mode: tb-rl;
  writing-mode: vertical-rl;
  font-size: 1rem;
  padding: 25px 20px;
  color: #9f886e;
  background-color: rgba(255, 255, 255, 0.7);
  cursor: pointer;
  -webkit-tap-highlight-color: #9f886e;
}
</style>
