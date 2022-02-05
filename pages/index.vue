<template>
  <div class="ApiTracking">
    <Header />

    <div class="container">
      <div class="row row-title">
        <div class="col offset-sm-2 offset-0">
          <h1 class="ml-sm-0 title">Api Tracking Routes</h1>
          <h3 class="ml-sm-0 subtitle">Choisissez une période pour commencer</h3>
        </div>
      </div>

      <div class="row">
        <div class="input-group mb-3">
          <input type="date" class="form-control" >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from '../components/Header.vue';
export default {
  components: { Header },
  data  () {
    return {
      api_data: null
    }
  }
  // async created () {
  //   const options = {
  //     contentType: 'application/json; charset=utf-8;',
  //     headers: {
  //       Authorization: 't8dwXjjIjrRq54LSq1Vt3SmWSYi9K7jbVzP5UdgD7ptPTJE5N0'
  //     }
  //   };
  //   const res = await fetch('http://api-buildings.homeys.io/api/tracking', options);
  //   this.api_data = await res.json();
  //   console.log(this.api_data);
  // }
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

}
</style>
