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
    data: Object
  },
  data() {
    return {
      chart: null,
      line_colors: ['#e21564','#1b3728','#751d7a','#213e66','#6d9b98','#1a5836','#1a5836','#3ace46','#9bc41b'],
      barChartData: {
        type: "line",
        data: {

          labels: ["Mercury", "Venus", "Earth", "Mars", "Jupiter", "Saturn", "Uranus", "Neptune"],
          datasets: [
            {
              data: [0, 0, 1, 2, 79, 82, 27, 14],
              fill: false,
              // borderColor: 'rgb(75, 192, 192)',
              tension: 0.2
            },
            {

              data: [0, 0, 1, 2, 79, 82, 27, 14],
              fill: false,
              // borderColor: 'rgb(75, 192, 192)',
              tension: 0.2
            }
          ]
        },
        options: {
          responsive: true,
          lineTension: 0,

          legend: {
            position: 'right',
          },
          title: {
            display: false
          },

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
  computed:{
    getRandomColor(){
      const hex = "0123456789ABCDEF";
      let color = '#';
      for (let i = 1; i<=6; i++) {
        color += hex[Math.floor(Math.random() * 16)];
      }
      return color
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
    if(this.data&&this.data.length>0){
      // this.barChartData.data.labels = this.labels
      // this.barChartData.data.datasets[0].data = this.datas
    }

    const ctx = document.getElementById('bar-chart');
    this.chart = new Chart(ctx, this.barChartData);
  }
}
</script>