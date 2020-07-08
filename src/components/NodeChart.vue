<template>
  <div>
    <highcharts :options="chartOptions" ref="chart2"></highcharts>
    <div class='test'>xx</div>
    <button @click="addNode">add node 4</button>
  </div>
</template>

<script>
import Highcharts from "highcharts"
import NetworkGraph from "highcharts/modules/networkgraph"
// import {Chart} from 'highlight-vue'
NetworkGraph(Highcharts)

export default {
  data() {
    return {
      nodes: [
              {
                id: 1,
                title: "node 1",
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
            dataLabels: {
              enabled: true,
              allowOverlap: true,
              linkFormat: "",
              format: "{point.title}",
            },
            marker: {
              radius: 30,
            },
            nodes: this.nodes,
            data: [
              {
                from: 1,
                to: 2,
                dataLabels: {
                  linkFormat: "1 -> 2",
                },
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
  computed: function(){
    console.log(this.chartOptions) 
    this.doThing();
    
  },
  mounted: function(){
    this.doThing();
  },
  methods: {
    doThing(){
      console.log('run')
    },
    addNode(){
      this.chartOptions.series.nodes.push({id:4, title:"node 4"})
    }
  }
}
</script>


<style lang="css" scoped>
* {
  margin: 1em;
}
tspan{
  background-color: red !important;
}
.test{
  background-color: red;
}
</style>