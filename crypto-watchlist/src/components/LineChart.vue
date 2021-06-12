<template>
  <div class="container">
    <div class="alert alert-warning" role="alert">
      Please note, I have hard-coded this graph using Canvas.js to test out how it works.
    </div>
    <canvas ref="chart-planet" />
  </div>
</template>

<script>
import Chart from 'chart.js';

export default {
  name: 'LineChart',
  data() {
    return {
      selectedYears: [2021]
    };
  },
  props: {
    chartType: {
      type: String,
      default: () => 'line'
    },
    chartData: {
      type: Object,
      default: () => {}
    },
    chartOptions: {
      type: Object,
      default: () => {}
    }
  },
  methods: {
    generateChart() {
      console.log('chart data',this.chartData)
      new Chart(this.$refs['chart-planet'], {
        type: this.chartType,
        data: this.chartData,
        options: this.chartOptions
      });
    }
  },
  mounted() {
    this.generateChart();
  }
}
</script>
