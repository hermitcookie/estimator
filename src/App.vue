<template>
  <div id="app">
   <v-app light>
    <v-toolbar color="white">
      <h1 style="font-family: Brush Script Std;">Estimators</h1>
    </v-toolbar>
    <v-content>
      <section>
        <v-parallax :src="require('../public/assets/wuhan-estimator.png')">
          <v-layout
            column
            align-center
            justify-center
            class="white--text"
          >
            <h1 class="white--text mb-2 display-1 font-weight-thin text-center">Number Of Death</h1>
            <vue-odometer :value="numOfDeath" format="d" theme="car" class="odometer" animation="smooth"></vue-odometer>
            <br>
            <h1 class="white--text mb-2 display-1 font-weight-thin text-center">Number Of Infected</h1>
            <vue-odometer :value="numOfInfected" theme="car" format="d" class="odometer" animation="smooth"></vue-odometer>
            <div class="subheading mb-4 font-weight-thin text-center">From Wuhan Coronavirus</div>
            <br>
            <h1 class="white--text mb-2 display-1 font-weight-thin text-center">{{this.currentWuhanTime}}, Wuhan, Hubei</h1>
            <!-- <div class="subheading mb-4 font-weight-thin text-center">Current Time in Wuhan</div> -->
            <!-- <vue-odometer :value="numOfInfected" theme="car" format="d" class="odometer" animation="smooth"></vue-odometer> -->
          </v-layout>
        </v-parallax>
      </section>

      <section>
        <v-layout
          column
          wrap
          class="my-12"
          align-center
        >
          <v-flex xs12 sm4 class="my-4">
            <div class="text-center">
              <h2 class="headline">Estimate Result of Wuhan Corona Virus!</h2>
              <!-- <span class="subheading">
                Last updated in 02-02-2020
              </span> -->
            </div>
          </v-flex>
          <v-flex xs12>
            <v-container grid-list-xl>
              <v-layout row wrap align-center>
                <v-flex xs12 md4>
                  <v-card flat class="transparent">
                    <v-card-text class="text-center">
                      <img width="64" src="../public/assets/death.png"/>
                    </v-card-text>
                    <v-card-title primary-title class="layout justify-center">
                      <div class="headline text-center">Number Of Death</div>
                    </v-card-title>
                    <date-picker v-on:emitDate="eventDeathPatcher"/>
                    <v-card-text class="display-3 font-weight-thin" align="center">
                      <vue-odometer :value="estimatedDeathNum" format="d" class="odometer" animation="smooth"></vue-odometer>
                    </v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs12 md4>
                  <v-card flat class="transparent">
                    <v-card-text class="text-center">
                      <img width="64" src="../public/assets/infection.png"/>
                    </v-card-text>
                    <v-card-title primary-title class="layout justify-center">
                      <div class="headline">Number Of Infected</div>
                    </v-card-title>
                    <date-picker v-on:emitDate="eventInfectedPatcher"/>
                    <v-card-text class="display-3 font-weight-thin" align="center">
                      <vue-odometer :value="estimatedInfectedNum" format="d" class="odometer" animation="smooth"></vue-odometer>
                    </v-card-text>
                  </v-card>
                </v-flex>
                <v-flex xs12 md4>
                  <v-card flat class="transparent" width="1000">
                    <v-card-text class="text-center">
                      <img src="../public/assets/lethality.png"/>
                    </v-card-text>
                    <v-card-title primary-title class="layout justify-center">
                      <div class="headline text-center">Lethality</div>
                    </v-card-title>
                    <v-card-text class="display-3 font-weight-thin" align="center">
                      {{ (this.record[this.record.length-1][0] / this.record[this.record.length-1][1] * 100).toFixed(2) }} %
                    </v-card-text>
                  </v-card>
                </v-flex>
              </v-layout>
            </v-container>
          </v-flex>
        </v-layout>
      </section>

      <!-- <section>
        <v-parallax :src="require('../public/assets/statistics2.jpg')" height="380">
          <v-layout column align-center justify-center>
            <div class="headline white--text mb-4 text-center">Check out other Estimators!</div>
            <em>Wanna know usage of plastic in 2030?</em>
            <v-btn
              class="mt-12"
              color="blue lighten-2"
              dark
              large
              href="/pre-made-themes"
            >
              Check other Estimators
            </v-btn>
          </v-layout>
        </v-parallax>
      </section>

      <section>
        <v-container grid-list-xl>
          <v-layout row wrap justify-center class="my-12">
            <v-flex xs12 sm4>
              <v-card flat class="transparent">
                <v-card-title primary-title class="layout justify-center">
                  <div class="headline">Estimate number of Death</div>
                </v-card-title>
                <RandomChart></RandomChart>
              </v-card>
            </v-flex>
            <v-flex xs12 sm4 offset-sm1>
              <v-card flat class="transparent">
                <v-card-title primary-title class="layout justify-center">
                  <div class="headline">Estimate number of Infected</div>
                </v-card-title>
                <RandomChart></RandomChart>
              </v-card>
            </v-flex>
          </v-layout>
        </v-container>
      </section> -->

      <v-footer color="blue darken-2">
        <v-layout row wrap align-center>
          <v-flex xs12>
            <div class="white--text ml-4">
              Please contact to by
              <b>worldestimator@gmail.com</b>,
              Jaden Wilson
            </div>
          </v-flex>
        </v-layout>
      </v-footer>
    </v-content>
  </v-app>
  </div>
</template>

<script>
import Vuetify from "vuetify";
import moment from 'moment';
import VueOdometer from "v-odometer/src";
import DatePicker from "./components/DatePicker.vue";
import { setTimeout } from "timers";

export default {
  name: "app",
  Vuetify: new Vuetify(),
  data() {
    return {
      title: "Estimator",
      numOfDeath: 0,
      numOfInfected: 0,
      duration: 0,
      today: moment().format('YYYY-MM-DD'),
      // record
      record: [
        [1, 41],
        [1, 41],
        [1, 41],
        [1, 41],
        [2, 45],
        [2, 45],
        [2, 62],
        [2, 121],
        [3, 198],
        [6, 291],
        [9, 440],
        [17, 571],
        [25, 830],
        [41, 1287],
        [56, 1975],
        [80, 2744],
        [106, 4515],
        [132, 5974],
        [170, 7711],
        [213, 9692],
        [259, 11791],
        [304, 14380],
        [361, 17205],
        [425, 20440],
        [490, 24324],
        [563, 28018],
        [637, 31161],
        [722, 34546],
        [811, 37198],
        [908, 40171],
        [1016, 42638],
        [1113, 44653],
        [1353, 59493],
        [1380, 63851], 
        [1523, 66492],
        [1665, 68500],
        [1827, 73764],
      ],
      newRecord: [],
      // estimated death num
      estimatedDeathNum: 0,
      estimatedInfectedNum: 0,
      avg_death_percent: 0,
      avg_infected_percent: 0,
      estimatedIncreaseDeathPerDay: 0,
      estimatedIncreaseInfectedPerDay: 0,
      currentWuhanTime: '',
    };
  },
  mounted() {
    clearTimeout();
    clearInterval();
    setInterval(this.updateTime, 100);
    this.estimatedDeathNum = this.estimateDeathPerDay();
    this.estimatedInfectedNum = this.estimateInfectedPerDay();
    this.estimatedIncreaseDeathPerDay = this.estimatedDeathNum - this.record[this.record.length-1][0];
    this.estimatedIncreaseInfectedPerDay = this.estimatedInfectedNum - this.record[this.record.length-1][1];
    this.startTime();
  },
  components: {
    "vue-odometer": VueOdometer,
    "date-picker": DatePicker
    // RandomChart,
  },
  destroyed() {
    console.log("destroyed");
    clearTimeout();
    clearInterval();
  },
  methods: {
    startTime() {
      let n = this.estimatedIncreaseDeathPerDay;
      let n2 = this.estimatedIncreaseInfectedPerDay;

      let time = new Date();
      let h = time.getHours();
      let m = time.getMinutes();
      let s = time.getSeconds();

      if (h < 8){
        h = h + 16;
      } else if (h == 8){
        h = 0;
      } else {
        h = h - 8;
      }

      h = h * 3600;
      m = m * 60;

      let current_time_in_second = h + m + s;

      let c = (current_time_in_second * n)/ 86400;
      let c2 = (current_time_in_second * n2)/ 86400;

      this.numOfDeath = this.record[this.record.length-1][0] + Math.round(c);
      this.numOfInfected = this.record[this.record.length-1][1] + Math.round(c2);

      console.log(
        "Num Of Death: ",
        this.numOfDeath,
        "Num Of Infected",
        this.numOfInfected
      );
      setTimeout(this.startTime, 500);
    },
    estimateDeathPerDay(){
      let estimatedDeathPerDay = 0;
      let increaseRecord = [0];
      let increasePercent = [];

      let sum_of_element_Record = 0;
      let sum_of_element_Percent = 0;

      for (var i = 1; i < this.record.length-1; i++) {
        console.log('running loop');
        increaseRecord[i] = (this.record[i+1][0] - this.record[i][0]);
        increasePercent[i-1] = increaseRecord[i]/increaseRecord[i-1]; 
      }
      for (let i = increaseRecord.length-20; i < increaseRecord.length; i++) {
        sum_of_element_Record += increaseRecord[i]; //length of increaseRecord = 20
        sum_of_element_Percent += increasePercent[i-1];
      }

      this.avg_death_percent = sum_of_element_Percent / 20;
      this.avg_death_number = sum_of_element_Record / 20;

      //linear square regression
      let x = 210; //sum of number 1-20 (20 days)
      let y = sum_of_element_Record; //sum of elements in increaseRecord
      let square_of_elem_Record = 0; //sum of x^2 where x = [1,20]
      let xy = 0; //sum of x*y
      for (let i = 1; i < 21; i++){
        xy = xy + i*increaseRecord[increaseRecord.length-21+i];
        square_of_elem_Record = square_of_elem_Record + i*i;
      }
      
      let m = (20*xy-x*y)/((20*square_of_elem_Record)-(x*x)); //m is the slope of best fit graph
      let b = (y - m*x)/20; //b is constant value
      let estimatedDeathToday = Math.round((m*21 + b) + (this.avg_death_number*this.avg_death_percent)); //average value of two estimated values

      estimatedDeathPerDay = this.record[this.record.length - 1][0] + estimatedDeathToday/2;
      console.log('about to return estimated death per day', estimatedDeathPerDay);
      return estimatedDeathPerDay;
    },
    estimateInfectedPerDay(){
      let estimatedInfectedPerDay = 0;
      let increaseRecord = [0];
      let increasePercent = [];      

      let sum_of_element_Record = 0;
      let sum_of_element_Percent = 0;

      for (let i = 1; i < this.record.length-1; i++) {
        increaseRecord[i] = (this.record[i+1][1] - this.record[i][1]);
        increasePercent[i-1] = increaseRecord[i]/increaseRecord[i-1]; 
      }
      for (let i = increaseRecord.length-10; i < increaseRecord.length; i++) {
        sum_of_element_Record += increaseRecord[i];
        sum_of_element_Percent += increasePercent[i-1];
      }
      this.avg_death_percent = sum_of_element_Percent / 10 - 0.15;
      this.avg_death_number = sum_of_element_Record / 10;

      //linear square regression
      let x = 210; //sum of number 1-20 (20 days)
      let y = sum_of_element_Record; //sum of elements in increaseRecord
      let square_of_elem_Record = 0; //sum of x^2 where x = [1,20]
      let xy = 0; //sum of x*y
      for (let i = 1; i < 21; i++){
        xy = xy + i*increaseRecord[increaseRecord.length-21+i];
        square_of_elem_Record = square_of_elem_Record + i*i;
      }
      
      let m = (20*xy-x*y)/((20*square_of_elem_Record)-(x*x)); //m is the slope of best fit graph
      let b = (y - m*x)/20; //b is constant value
      let estimatedDeathToday = Math.round((m*21 + b) + (this.avg_death_number*this.avg_death_percent)); //average value of two estimated values

      estimatedInfectedPerDay = this.record[this.record.length - 1][1] + estimatedDeathToday/2;
      return estimatedInfectedPerDay;
    },
    calculateDeath(days){
      let estimatedDeathPerDay = this.estimateDeathPerDay();
      for (let k = 1; k <= days; k++){
        estimatedDeathPerDay = estimatedDeathPerDay + Math.round(this.avg_death_number*this.avg_death_percent);
      }
      this.estimatedDeathNum = estimatedDeathPerDay;
    },
    calculateInfected(days){
      let estimatedInfectedPerDay = this.estimateInfectedPerDay();
      for (let k = 1; k <= days; k++){
        estimatedInfectedPerDay = estimatedInfectedPerDay + Math.round(this.avg_death_number*this.avg_death_percent);
      }
      this.estimatedInfectedNum = estimatedInfectedPerDay;
    },
    eventDeathPatcher(date) {
      this.calculateDeath(1+Math.round((new Date(date).getTime() - new Date().getTime()) / 1000 / 86400));
    },
    eventInfectedPatcher(date) {
      this.calculateInfected(1+Math.round((new Date(date).getTime() - new Date().getTime()) / 1000 / 86400));
    },
    updateTime() {
      this.currentWuhanTime = moment().zone("+08:00").format("MM-DD-YYYY, h:mm:ss A")
    }
  }
};
</script>

<style>
.odometer {
  font-size: 50px;
}
</style>
