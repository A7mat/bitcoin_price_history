<template>
  <div id="app" class="container align-items-start">
    <div class="row mt-5">
      <div class="col text-center">
        <h1>Bitcoin Price Tracker</h1>
      </div>
    </div>
    <form class="row m-5">
      <div class="col-5">
        <label for="fromDate">From:</label>
        <input
          type="date"
          v-model="fromDate"
          id="fromDate"
          class="form-control"
        />
      </div>
      <div class="col-5">
        <label for="to">To:</label>
        <input type="date" v-model="toDate" id="toDate" class="form-control" />
      </div>
      <div class="raw">
        <input
          type="button"
          class="btn btn-outline-secondary mt-2"
          value="Render"
          @click="updateData()"
        />
      </div>
    </form>
    <section v-if="errored">
      <p class="alert alert-danger" role="alert">
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
      fromDate: this.getTenDaysAgo(),
      toDate: this.getToday(),
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
                label: "Price in USD",
                backgroundColor: "rgba(242, 169, 0, 0.3)",
                borderColor: "#4d4d4e",
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
    getToday() {
      return new Date().toISOString().slice(0, 10);
    },
    getTenDaysAgo() {
      let today = new Date();
      today.setDate(today.getDate() - 10);
      return today.toISOString().slice(0, 10);
    },
  },
};
</script>