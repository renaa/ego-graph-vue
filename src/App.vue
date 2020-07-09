<template>
  <div id="app">
    
    <!-- <highcharts class="hc" :options="chartOptions"></highcharts> -->
    <!-- <highcharts
      :constructorType="'ganttChart'"
      class="hc"
      :options="chartOptions"
      ref="chart"
    ></highcharts> -->
    <Query @result='onDataFromServer' @searching='searchingAnimation'/>
      <div id='q'>test</div>
    <NodeChart :chartOptions='chartOptions'/>
  </div>
</template>

<script>
import Query from './components/Query' 

import NodeChart from './components/NodeChart'

// import { networkgraph } from "highcharts-vue"
// import Highcharts from "highcharts"
// import exportingInit from "highcharts/modules/exporting"

// exportingInit(Highcharts)

export default {
  name: "App",
  components: {
    Query,

    NodeChart,
  },
  data: function() {
    return {
      response: "",
      chartOptions: {
        chart: {
          type: "networkgraph",
        },
        title: false,
        credits: false,
        xAxis: {
          title: false,
        },
        yAxis: {
          title: false,
        },
        series: [
          {
            layoutAlborithm: {
              enableSimulation: true
            },
            dataLabels: {
              enabled: true,
              allowOverlap: true,
              linkFormat: "",
              format: "{point.title}",
            },
            marker: {
              radius: 30,
            },
            nodes: [
              {
                id: 1,
                title: "node 1",
                color: this.c
              },
              {
                id: 2,
                title: "node 2",
              },
              {
                id: 3,
                title: "node 3",
              },
            ],
            data: [
              {
                from: 1,
                to: 2,
                dataLabels: {
                  linkFormat: "1 -> 2",
                },
                lineWidth: 3
              },
              {
                from: 1,
                to: 3,
                dataLabels: {
                  linkFormat: "1 -> 3",
                },
              },
              {
                from: 2,
                to: 3,
                dataLabels: {
                  linkFormat: "2 -> 3",
                },
              },
            ],
          },
        ],
      },
    }
  },
  methods: {
    onDataFromServer(value){
      this.response = value
      this.chartOptions.series[0].nodes = value.Item1
      this.chartOptions.series[0].data = value.Item2
    },
    searchingAnimation(){
        document.getElementById('highChart').style.background = 'red';
        document.getElementById('q').style.background = 'red';
    }
  }
  
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
