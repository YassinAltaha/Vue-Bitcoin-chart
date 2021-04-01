<template>
  <div class="small">
    <h1>Powered by CoinDesk</h1>

    <line-chart v-if="loaded" :chart-data="datacollection"></line-chart>
    <h1 v-else>Loading Bitcoin Price</h1>
  </div>
</template>

<script>
import LineChart from "./mixin/LineChart.js";

export default {
  name: "Bitcoin Chart",
  components: {
    LineChart,
  },
  data() {
    return {
      loaded: false,
      datacollection: null,
      totalVuePackages: Object,
    };
  },
  async created() {
    //Fetching Bitcoin data from coindesk api and allocating data into variable
    fetch("https://api.coindesk.com/v1/bpi/historical/close.json")
      .then(async (response) => {
        const data = await response.json();

        // check for error response
        if (!response.ok) {
          // get error message from body or default to response statusText
          const error = (data && data.message) || response.statusText;
          return Promise.reject(error);
        }
        console.log("Created " + this.totalVuePackages);
        this.totalVuePackages = data.bpi;

        this.fillData();
        this.loaded = true;
      })
      .catch((error) => {
        this.errorMessage = error;
        console.error("There was an error!", error);
      });
  },

  methods: {
    fillData() {
      this.datacollection = {
        labels: Object.keys(this.totalVuePackages),
        datasets: [
          {
            label: "Bitcoin Price",
            backgroundColor: "#f87939",
            data: Object.values(this.totalVuePackages),
          },
        ],
      };
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.small {
  max-width: 600px;
  margin: 150px auto;
}
</style>
