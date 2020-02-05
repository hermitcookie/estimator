<template>
  <div id="app">
   <v-app light>
    <v-toolbar color="white">
      <h1 style="font-family: Brush Script Std;">Estimator-junyoungs</h1>
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
            <vue-odometer :value="numOfInfected" theme="car" format="d" class="odometer"></vue-odometer>
            <div class="subheading mb-4 font-weight-thin text-center">From Wuhan Coronavirus</div>
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
              <h2 class="headline">Summary of Wuhan Corona Virus</h2>
              <span class="subheading">
                Last updated in 02-02-2020
              </span>
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
                    <v-card-text class="display-3 font-weight-thin" align="center">
                      109
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
                    <v-card-text class="display-3 font-weight-thin" align="center">
                      6719
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
                      2.2%
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
              Please contact to 
              by <a class="white--text" href="https://vuetifyjs.com" target="_blank">jwhan0316@gmail.com</a>
              Junyoung Park
            </div>
          </v-flex>
        </v-layout>
      </v-footer>
    </v-content>
  </v-app>
  </div>
</template>

<script>
import Vuetify from 'vuetify'
import VueOdometer from 'v-odometer/src'
import { setTimeout } from 'timers';

export default {
  name: 'app',
  Vuetify: new Vuetify(),
  data () {
    return {
      title: 'Estimator',
      numOfDeath: 0,
      numOfInfected: 0,
      duration: 0,
      // junyoung's data
      n: 60,
      x: 421,
      n2: 2500,
      x2: 16880,
    }
  },
  mounted() {
    this.startTime();
  },
  components: {
    'vue-odometer': VueOdometer,
    // RandomChart,
  },
  beforeDestroy() {
    console.log('Clearing setTimeOut');
    clearTimeout();
  },
  methods: {
    startTime() {
      let td = 86400 / this.n;
      let ti = 86400 / this.n2;
      let today = new Date();
      let h = today.getHours();
      let m = today.getMinutes();
      let s = today.getSeconds();

      h = h + 16;
      if (h >= 24) {h = h - 24}

      h = h * 3600; 
      m = m * 60;

      let current_time_in_second = h + m + s;

      let c = current_time_in_second/td;
      let c2 = current_time_in_second/ti;

      this.numOfDeath = this.x + Math.round(c);
      this.numOfInfected = this.x2 + Math.round(c2);  

      console.log("Num Of Death: ", this.numOfDeath, "Num Of Infected", this.numOfInfected);

      setTimeout(this.startTime, 500);
    },
    checkTime(i) {
      if (i < 10) {
        i = "0" + i;
        return i;
      }
    }
  }
}
</script>

<style>
.odometer {
  font-size: 50px;
}
</style>
