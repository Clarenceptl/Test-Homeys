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
      line_color: null,
      line_colors: ['#e21564','#1b3728','#751d7a','#213e66','#6d9b98','#1a5836','#1a5836','#3ace46','#9bc41b'],
      barChartData: {
        type: "line",
        data: {
          labels: [],
          datasets: [
            // {
            //   data: [0, 0, 1, 2, 79, 82, 27, 14],
            //   fill: false,
            //   borderColor: 'rgb(75, 192, 192)',
            //   tension: 0.2
            // }
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
                }
              }
            ],
            xAxes: [
              {
                ticks: {

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
  computed:{

  },
  watch: {
    deep: true,
    immediate: true,
    data(val, oldVal) {
      this.barChartData.data = val
      this.setChartDataOptions();
      this.change();
    }
  },
  methods: {
    getRandomColor(){
      const hex = "0123456789ABCDEF";
      let color = '#';
      for (let i = 1; i<=6; i++) {
        color += hex[Math.floor(Math.random() * 16)];
      }
      this.color = color;
    },
    change(){
      const ctx = document.getElementById('lines-chart');
      this.chart.update()
    },
    setChartDataOptions(){
      this.barChartData.data.datasets = this.barChartData.data.datasets.map((item)=>{
        this.getRandomColor();
        return {...item,fill: false,borderColor: this.color, tension: 0.2}
      });
    }
  },
  mounted() {
    if(this.data && Object.keys(this.data).length > 0){
      this.barChartData.data = this.data
      this.setChartDataOptions();
    }

    const ctx = document.getElementById('lines-chart');
    this.chart = new Chart(ctx, this.barChartData);
  }
}
</script>