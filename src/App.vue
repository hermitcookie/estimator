<template>
  <div id="app">
   <v-app light>
    <v-toolbar color="white">
      <h1>Coronavirus Estimator</h1>
    </v-toolbar>
    <v-content>
      <section>
        <v-parallax :src="require('../public/assets/mask.png')">
          <v-layout
            column
            align-center
            justify-center
            class="black--text"
          >
            <h1 class="black--text mb-2 display-1 font-weight-thin text-center">Number Of Death, Worldwide</h1>
            <vue-odometer :value="numOfDeath" format="d" theme="car" class="odometer" animation="smooth"></vue-odometer>
            <br>
            <h1 class="black--text mb-2 display-1 font-weight-thin text-center">Number Of Infected, Worldwide</h1>
            <vue-odometer :value="numOfInfected" theme="car" format="d" class="odometer" animation="smooth"></vue-odometer>
            <div class="subheading mb-4 text-center">From Coronavirus</div>
            <br>
            <h1 class="black--text mb-2 display-1 font-weight-thin text-center">{{this.currentWuhanTime}}</h1>
            <div class="subheading mb-4 text-center">New York City, NY, United States America</div>
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
              <h2 class="headline">Predict The Result With Estimator!</h2>
              <span class="subheading">
                Based on the Method of <a href="https://en.wikipedia.org/wiki/Least_squares" style="font-weight: bold; color: black; text-decoration: none;">Least Squares</a>
              </span>
            </div>
          </v-flex>
          <v-flex xs12>
            <v-container grid-list-xl>
              <v-layout row wrap align-center>
                <v-flex xs12 md3>
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
                                <v-flex xs12 md3>
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
                <v-flex xs12 md3>
                  <v-card flat class="transparent" width="1000">
                    <RandomChart color="#fada5e" label="Infected" :record="this.InfectedRecord"></RandomChart>
                  </v-card>
                </v-flex>
                <v-flex xs12 md3>
                  <v-card flat class="transparent" width="1000">
                    <RandomChart color="#f87979" label="Death" :record="this.DeathRecord"></RandomChart>
                  </v-card>
                </v-flex>
              </v-layout>
            </v-container>
          </v-flex>
        </v-layout>
      </section>

      <v-footer color="blue darken-2">
        <v-layout row wrap align-center>
          <v-flex xs12>
            <div class="white--text ml-4">
              Please contact to by
              <b>coronaestimator@gmail.com</b>
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
import RandomChart from './components/RandomChart.vue';

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
      // [Name of Country, number of infected, number of death]
      // world: [
      //   ['China',	74577,	2118],
      //   ['Diamond Princess',	621,	2],
      //   ['Japan',	85,	1],
      //   ['Singapore',	84, 0	],
      //   ['S. Korea',	82,	0 ],
      //   ['Hong Kong',	65,	2],
      //   ['Thailand',	35, 0],
      //   ['Taiwan',	23,	1],
      //   ['Malaysia',	22, 0],
      //   ['Germany',	16,	0],
      //   ['Vietnam',	16, 0],
      //   ['Australia',	15,	0],
      //   ['USA',	15,	0],
      //   ['France',	12,	1],
      //   ['Macao',	10,	0],
      //   ['U.K.',	9, 0],
      //   ['U.A.E.',	9,	0],
      //   ['Canada',	8,	0],
      //   ['Philippines',	3,	1],
      //   ['India',	3,	0],
      //   ['Italy',	3,	0],
      //   ['Iran',	2,	2],
      //   ['Russia',	2,	0],
      //   ['Spain',	2,	0],
      //   ['Belgium',	1,	0],
      //   ['Cambodia',	1,	0],
      //   ['Egypt',	1,	0],
      //   ['Finland',	1,	0],
      //   ['Nepal',	1,	0],
      //   ['Sri Lanka',	1,	0],
      //   ['Sweden',	1,	0],
      // ],
      //record, this starts from FEB 22 to MAR 14
      record: [
        [580, 17],
        [845, 25],
        [1317, 41],
        [2015, 56],
        [2800, 80],
        [4581, 106],
        [6058, 132],
        [7813, 170],
        [9823, 213],
        [11950, 259],
        [14553, 304],
        [17391, 362],
        [20630, 426],
        [24545, 492],
        [28266, 638],
        [31439, 724],
        [34876, 813],
        [37552, 910],
        [40553, 1018],
        [43099, 1115],
        [45134, 1261],
        [59287, 1383],
        [64438, 1526],
        [67100, 1669],
        [69197, 1775],
        [71329, 1873],
        [73332, 2009],
        [75700, 2126],
        [76677, 2247],
        [77673, 2360],
        [78651, 2460],
        [79205, 2618],
        [80087, 2699],
        [80828, 2763], 
        [81820, 2800],
        [83112, 2858],
        [84615, 2923],
        [86604, 2977],
        [88585, 3050],
        [90443, 3117],
        [93016, 3202],
        [95314, 3285],
        [98425, 3387],
        [102050, 3494],
        [106099, 3599],
        [109991, 3827],
        [114381, 4025],
        [118948, 4296],
        [126214, 4628],
        [134509, 4981],
        [145416, 5429],
        [156475, 5833],
        [169511, 6520],
        [182431, 7162],
        [198159, 7979],
        [218744, 8951],
        [244902, 10030],
        [275550, 11386],
        [304979, 13011],
        [337459, 14640],
        [378830, 16513],
        [422574, 18894],
        [471035, 21282],
        [531865, 24073],
        [596366, 27343],
        [663127, 30861],
        [723390, 34065],
        [784741, 37774],
        [858361, 42309],
        [935232, 47198],
        [1015096, 53177],
        [1116662, 58891],
        [1201483, 64690],
        [1272901, 69427],
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
      InfectedRecord: [],
      DeathRecord: [],
    };
  },
  mounted() {
    clearTimeout();
    clearInterval();
    setInterval(this.updateTime, 100);
    this.setData();
    this.estimatedDeathNum = this.estimateDeathPerDay();
    this.estimatedInfectedNum = this.estimateInfectedPerDay();
    this.estimatedIncreaseDeathPerDay = this.estimatedDeathNum - this.record[this.record.length-1][1];
    this.estimatedIncreaseInfectedPerDay = this.estimatedInfectedNum - this.record[this.record.length-1][0];
    this.startTime();
  },
  components: {
    "vue-odometer": VueOdometer,
    "date-picker": DatePicker,
    RandomChart,
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

      this.numOfDeath = this.record[this.record.length-1][1] + Math.round(c);
      this.numOfInfected = this.record[this.record.length-1][0] + Math.round(c2);

      console.log(
        "Num Of Death: ",
        this.numOfDeath,
        "Num Of Infected",
        this.numOfInfected
      );
      setTimeout(this.startTime, 5000);
    },
    estimateDeathPerDay(){
      let estimatedDeathPerDay = 0;
      let increaseRecord = [0];
      let increasePercent = [];

      let sum_of_element_Record = 0;
      let sum_of_element_Percent = 0;

      for (var i = 1; i < this.record.length-1; i++) {
        increaseRecord[i] = (this.record[i+1][1] - this.record[i][1]);
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

      estimatedDeathPerDay = this.record[this.record.length - 1][1] + estimatedDeathToday/2;
      return estimatedDeathPerDay;
    },
    estimateInfectedPerDay(){
      let estimatedInfectedPerDay = 0;
      let increaseRecord = [0];
      let increasePercent = [];      

      let sum_of_element_Record = 0;
      let sum_of_element_Percent = 0;

      for (let i = 1; i < this.record.length-1; i++) {
        increaseRecord[i] = (this.record[i+1][0] - this.record[i][0]);
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

      estimatedInfectedPerDay = this.record[this.record.length - 1][0] + estimatedDeathToday/2;
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
      this.currentWuhanTime = moment().zone("-04:00").format("MM-DD-YYYY, h:mm:ss A")
    },
    setData() {
      for (let i = 0; i < this.record.length; i++) {
        this.InfectedRecord.push(this.record[i][0]);
        this.DeathRecord.push(this.record[i][1]);
      }
    }
  }
};
</script>

<style>
.odometer {
  font-size: 50px;
}
</style>
