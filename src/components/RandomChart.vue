<template>
  <div>
    <line-chart :chart-data="datacollection"></line-chart>
    <!-- <button @click="fillData()">Randomize</button> -->
  </div>
</template>

<script>
  import LineChart from './LineChart.js'

  export default {
    components: {
      LineChart
    },
    data () {
      return {
        datacollection: null,
        // InfectedRecord: [],
        // DeathRecord: [],
      }
    },
    props: {
      record: null,
      color: null,
      label: null,
    },
    mounted () {
      // set the array 
      // this.setData();
      this.fillData();
    },
    methods: {
      fillData () {
        this.datacollection = {
          labels: ['1/22', '1/23', '1/24', '1/25', '1/26', '1/27', '1/28', '1/29', '1/30', '1/31', '2/1', '2/2', '2/3', '2/4', '2/5', '2/6',
                    '2,7', '2/8', '2/9', '2/10', '2/11', '2/12', '2/13', '2/14', '2/15', '2/16', '2/17', '2/18', '2/19', '2/20', '2/21', '2/22', '2/23',
                    '2/24', '2/25', '2/26', '2/27', '2/28', '2/29', '3/1', '3/2', '3/3', '3/4', '3/5', '3/6', '3/7', '3/8', '3/9', '3/10', '3/11', '3/12'
                    ,'3/13'],
          datasets: [
            {
              label: this.label,
              backgroundColor: this.color,
              data: this.record
            }
          ]
        }
      },
    }
  }
</script>

<style>
  .small {
    max-width: 600px;
    margin:  150px auto;
  }
</style>