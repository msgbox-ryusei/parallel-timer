<template>
  <v-app>
    <v-main>
      <div class="contant">
        <v-dialog v-model="dialog" width="500">
          <template v-slot:activator="{ on, attrs }">
            <v-btn color="blue lighten-2" dark v-bind="attrs" v-on="on" class="mx-3 mt-2">
              Add Timer
            </v-btn>
          </template>

          <v-card>
            <v-card-title class="text-h5 grey lighten-2"> Add Timer </v-card-title>

            <v-card-text>
              <v-container>
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
              <v-btn color="primary" text @click="dialog = false"> Cancel </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <Timer
          v-for="timer of timers"
          :key="timer.id"
          class="ma-3"
          :time="timer.time"
        ></Timer>
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
    dialog: false,
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
      console.log(sum);
      if (sum !== 0) {
        const timer = {
          time: sum,
          id: this.timerIdSequence,
        };
        this.timers.unshift(timer);
        this.timerIdSequence++;
        this.dialog = false;
      }
    },
  },
};
</script>
<style>
.content {
  margin: 0 auto;
}
</style>
