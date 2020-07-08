<template>
  <div>
    <highcharts :constructorType="'stockChart'" class="hc" :options="chartOptions" ref="chart"></highcharts>
  </div>
</template>

<script>
import Highcharts from 'highcharts'
import Stock from 'highcharts/modules/stock'
Stock(Highcharts)

export default {
  data() {
    return {
      chartOptions: {
        series: [
          {
            data: [1, 2, 3]
          }
        ]
        
      }
    };
  }
};
</script>
