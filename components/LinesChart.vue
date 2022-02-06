<template>
  <div>
    <canvas id="lines-chart"></canvas>
  </div>
</template>

<script>

import Chart from 'chart.js'

export default {
  name: 'LinesChart',
  props:{
    labels: Array,
    datas: Array
  },
  data() {
    return {
      chart: null,
      barChartData: {
        type: "line",
        data: {
          labels: [],
          // labels: ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"],
          datasets: [
            {
              data: [],
              // data: [0, 0, 1, 2, 79, 82, 27, 14],
              fill: false,
              borderColor: 'rgb(75, 192, 192)',
              tension: 0.1
            }
          ]
        },
        options: {
          responsive: true,
          lineTension: 0,
          scales: {
            yAxes: [
              {
                ticks: {
                  beginAtZero: true,
                  padding: 25
                }
              }
            ]
          }
        }
      }
    }
  },
  watch: {
    deep: true,
    immediate: true,
    datas(val, oldVal) {
      this.barChartData.data.datasets[0].data = val
      this.change()
    },
    labels(val, oldVal){
      this.barChartData.data.labels = val
      this.change()
    }
  },
  methods: {
    change(){
      const ctx = document.getElementById('bar-chart');
      this.chart.update()
    }
  },
  mounted() {
    this.barChartData.data.labels = this.labels
    this.barChartData.data.datasets[0].data = this.datas
    const ctx = document.getElementById('bar-chart');
    this.chart = new Chart(ctx, this.barChartData);
  }
}
</script>