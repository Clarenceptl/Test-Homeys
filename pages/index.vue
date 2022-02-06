<template>
  <div class="ApiTracking">
    <Header />

    <div class="container">
      <div class="row row-title">
        <div class="col offset-sm-2 offset-0">
          <h1 class="ml-sm-0 title">Api Tracking Routes</h1>
          <h2 class="ml-sm-0 subtitle">Choisissez une période pour commencer</h2>
        </div>
      </div>

      <div class="row mb-3">
        <div class="input-group mb-3">
          <div class="col">
            <label for="begin">Début</label>
            <input type="date" name="begin" id="begin"  class="form-control" @change="dataFilter()" v-model="form_value_begin"  min="2019-01-01">
          </div>
          <div class="col">
            <label for="end">Fin</label>
            <input type="date" name="end" id="end" placeolder="2018-01-01" class="form-control" @change="dataFilter()" v-model="form_value_end" :min="form_value_begin">
          </div>
        </div>
      </div>

      <div class="row d-flex justify-content-center card-stats mb-4">
        <div class="card shadow" style="width: 22rem;">
          <div class="card-body">
            <h4 class="card-title text-center mb-2">Total d'appels</h4>
            <p class="card-subtitle text-center">{{callNb}}</p>
            <h4 class="card-title text-center mt-4 mb-2">Période</h4>
            <p class="card-subtitle text-center">{{form_value_begin}} / {{form_value_end}}</p>
          </div>
        </div>
      </div>
      <BarChart :data="barChartData"/>
    </div>
  </div>
</template>

<script>
import Header from '../components/Header.vue';
import BarChart from '~/components/BarChart.vue';

export default {
  components: { Header, BarChart },
  data  () {
    return {
      form_value_begin: "2021-12-01",
      form_value_end: "2021-12-05",
      api_data: null,
      api_data_filtered: null,
      chart_bar_labels: [],
      chart_bar_data: []
    }
  },
  methods: {
    async getData(){
      try {
        const options = {
          contentType: 'application/json; charset=utf-8;',
          headers: {
            Authorization: 't8dwXjjIjrRq54LSq1Vt3SmWSYi9K7jbVzP5UdgD7ptPTJE5N0'
          }
        };
        const res = await fetch('http://api-buildings.homeys.io/api/tracking', options);
        this.api_data = await res.json();
      } catch (error) {
        console.error(error)
      }
    },
    dataFilter(){
      let start = new Date(this.form_value_begin);
      let end = new Date(this.form_value_end);
      if(this.api_data){
        this.api_data_filtered = this.api_data.filter(item => {
          let date = new Date(item.date);
          return date >= start && date <= end;
        })
      }

    },


  },
  computed: {
    barChartData(){
      if(this.api_data_filtered){
        let chart_data = [{label:[],data:[]}];
        let filter_chart_arr = [];
        let check_date_arr = [];

        for (let item of this.api_data_filtered) {
          if(check_date_arr.indexOf(item.date)> -1 ){
            filter_chart_arr[check_date_arr.indexOf(item.date)].count += item.count
          }else{
            check_date_arr.push(item.date);
            filter_chart_arr.push(item)
          }
        }
        for (let item of filter_chart_arr) {
          chart_data[0].label.push(item.date);
          chart_data[0].data.push(item.count);
        }
        return chart_data
      }
      return null
    },
    callNb(){
      let call_nb=0;
      if(this.api_data_filtered){
        this.api_data_filtered.map((item)=>{
          call_nb += item.count
        });
      }

      return call_nb
    }
  },

  async created () {
    await this.getData();
    this.dataFilter()
    // console.log(this.api_data_filtered,'create')
  }
}
</script>
<style scoped lang="scss">
.ApiTracking{
  .row-title{
    margin-bottom: 80px;
    .title{
      font-weight: 300;
    }
    .subtitle{
      font-weight: 300;
      color: grey;
      font-size: 1.2rem;
    }
  }
  .card-stats{
    .card{
      background-color: $card-stats;
      .card-title{
        font-size: 1rem;
        color:  lighten($color: $card-title, $amount: 10);
      }
      .card-subtitle{
        font-size: .875rem;
        color: white;
      }
    }

  }

}
</style>
