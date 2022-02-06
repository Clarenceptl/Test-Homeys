<template>
  <div>
    <canvas id="bar-chart"></canvas>
  </div>
</template>

<script>

import Chart from 'chart.js'

export default {
  name: 'barChart',
  props:{
    data: Array
  },
  data() {
    return {
      chart: null,
      barChartData: {
        type: "bar",
        data: {
          labels: [],
          // labels: ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"],
          datasets: [
            {
              data: [],
              // data: [0, 0, 1, 2, 79, 82, 27, 14],
              backgroundColor: "#304282",
              borderWidth: 0
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
    data(val, oldVal) {
      this.barChartData.data.datasets[0].data = val[0].data
      this.barChartData.data.labels = val[0].label
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
    if(this.data && this.data.length > 0){
      this.barChartData.data.labels = this.data[0].label
      this.barChartData.data.datasets[0].data = this.data[0].data
    }

    const ctx = document.getElementById('bar-chart');
    this.chart = new Chart(ctx, this.barChartData);


  }
}
</script>