<template>
  <div id="app container">
    <div class="row mt-5">
      <div class="col text-center">
        <h1>Bitcoin Price Tracker</h1>
      </div>
    </div>
    <div class="row m-5">
      <div class="col">
        <input type="date" v-model="fromDate">
      </div>
      <div class="col">
        <input type="date" v-model="toDate">
      </div>
      <div class="col">
        <input type="button" value="render" @click="updateData()">
      </div>
    </div>
    <div class="row m-5">
      {{data}}
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {
  },
  data(){
    return {
      fromDate: "2022-01-01",  // To-Do: shouldn't be hardcoded
      toDate: "2022-01-31",
      data: null,
    }
  },
  mounted () {
    axios
      .get(`https://api.coindesk.com/v1/bpi/historical/close.json?start=${this.fromDate}&end=${this.toDate}&index=[USD]`)
      .then(response => (this.data = response.data.bpi))
      .catch(error => console.log(error))

  },
  methods: {
    updateData(){
      axios
      .get(`https://api.coindesk.com/v1/bpi/historical/close.json?start=${this.fromDate}&end=${this.toDate}&index=[USD]`)
      .then(response => (this.data = response.data.bpi))
      .catch(error => console.log(error))
    }
  }
}
</script>

<style>

</style>
