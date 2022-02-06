<template>
  <div id="app container">
    <div class="row mt-5">
      <div class="col text-center">
        <h1>Bitcoin Price Tracker</h1>
      </div>
    </div>
    <div class="row m-5">
      <div class="col">
        <input type="date" v-model="fromDate" />
      </div>
      <div class="col">
        <input type="date" v-model="toDate" />
      </div>
      <div class="col">
        <input type="button" value="render" @click="updateData()" />
      </div>
    </div>

    <section v-if="errored">
      <p>
        We're sorry, we're not able to retrieve this information at the moment,
        please refresh the page and try again later
      </p>
    </section>

    <section v-else>
      <div v-if="loading">Loading...</div>

      <div v-else class="row m-5">
        <price-chart
          :chartData="data"
          :options="chartOptions"
          label="Bitcoin"
        ></price-chart>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import PriceChart from "./components/PriceChart.vue";

export default {
  name: "App",
  components: {
    PriceChart,
  },
  data() {
    return {
      fromDate: "2022-01-01", // To-Do: shouldn't be hardcoded
      toDate: "2022-01-31",
      data: null,
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
      },
      loading: true,
      errored: false,
    };
  },
  mounted() {
    this.updateData();
  },
  methods: {
    updateData() {
      axios
        .get(
          `https://api.coindesk.com/v1/bpi/historical/close.json?start=${this.fromDate}&end=${this.toDate}&index=[USD]`
        )
        .then((response) => {
          this.data = {
            labels: Object.keys(response.data.bpi),
            datasets: [
              {
                label: "Bitcoin",
                backgroundColor: "#f87979",
                data: Object.values(response.data.bpi),
              },
            ],
          };
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })
        .finally(() => (this.loading = false));
    },
  },
};
</script>