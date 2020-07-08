<template>
  <div id="app">
    
    <!-- <highcharts class="hc" :options="chartOptions"></highcharts> -->
    <!-- <highcharts
      :constructorType="'ganttChart'"
      class="hc"
      :options="chartOptions"
      ref="chart"
    ></highcharts> -->
    <Query @myEmit='onDataFromServer'/>
    <GanttChart/>
    <StockChart/>
    <NodeChart/>
  </div>
</template>

<script>
import Query from './components/Query' 
import GanttChart from './components/GanttChart'
import StockChart from './components/StockChart'
import NodeChart from './components/NodeChart'

// import { networkgraph } from "highcharts-vue"
// import Highcharts from "highcharts"
// import exportingInit from "highcharts/modules/exporting"

// exportingInit(Highcharts)

  "https://uniquefunctionname.azurewebsites.net"
export default {
  name: "App",
  components: {
    Query,
    GanttChart,
    StockChart,
    NodeChart,
  },
  data: function() {
    return {
      response: "",
      chartOptions: {
     
        
      },
    }
  },
  methods: {
    onDataFromServer(value){
      this.response = value
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
