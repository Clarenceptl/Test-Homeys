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
          datasets: [
            {
              data: [],
              backgroundColor: "#304282",
            }
          ]
        },
        options: {
          responsive: true,
          legend: {
            display: false
          },
          title: {
            display: false
          },

          scales: {
            yAxes: [
              {
                ticks: {
                  beginAtZero: true,
                },
                gridLines:{
                  display: false
                },

              }
            ],
            xAxes: [
              {
                ticks: {
                  beginAtZero: true,
                  padding: 10
                },
                gridLines:{
                  display: false
                },

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
    console.log(this.barChartData)

    const ctx = document.getElementById('bar-chart');
    this.chart = new Chart(ctx, this.barChartData);


  }
}
</script>