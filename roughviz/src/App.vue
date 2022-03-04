<template>
  <div id="app">
    <rough-pie
      v-if="chartValue.length > 0"
      :data="{
        labels: chartLabel,
        values: chartValue,
      }"
      title="BTC - 10 Days"
      roughness="4"
      color="#ccc"
      stroke="black"
      stroke-width="1"
      fill-style="zigzag-line"
      fill-weight="2"
      legendPosition="left"
    />
  </div>
</template>

<script>
import axios from "axios";
import { RoughPie } from "vue-roughviz";
export default {
  name: "App",
  components: {
    RoughPie,
  },
  data() {
    return {
      chartLabel: [],
      chartValue: [],
    };
  },
  mounted() {
    this.loadData();
  },
  methods: {
    loadData() {
      axios
        .get(
          "https://api.coingecko.com/api/v3/coins/bitcoin/market_chart?vs_currency=usd&days=10&interval=daily"
        )
        .then((res) => {
          res.data.prices.map((v) => {
            let date = new Date(v[0]).toDateString();
            let rPrice = v[1];
            this.chartLabel.push(date);
            this.chartValue.push(rPrice);
          });
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
