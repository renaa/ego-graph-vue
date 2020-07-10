<template>
  <div>
    <highcharts :options="chartOptions" ref="chart2"></highcharts>
    <button @click="addNode">add node 4</button>
  </div>
</template>

<script>
import Highcharts from "highcharts"
import NetworkGraph from "highcharts/modules/networkgraph"
// import {Chart} from 'highlight-vue'
NetworkGraph(Highcharts)

export default {
  props: ['chartOptions'],
  data() {
    return {
      
      c: '#ff00ff',
      
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
      // this.chartOptions.series[0].nodes = this.n
      // this.chartOptions.series[0].data = this.d
    }
  }
}
</script>


<style lang="css" scoped>
* {
  margin: 1em;
}

</style>