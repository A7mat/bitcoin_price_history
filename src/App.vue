<template>
  <div id="app">

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
      fromDate: "2021-12-31",
      toDate: "2022-01-31",
    }
  },
  async created(){
    const { data } = await axios.get(`https://api.coindesk.com/v1/bpi/historical/close.json?start=${this.fromDate}&end=${this.toDate}&index=[USD]`);
    console.log(data);
  }
}
</script>

<style>

</style>
