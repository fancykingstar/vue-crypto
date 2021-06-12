<template>
<div class="detailed">
    <h3>{{ name }}</h3>
    <br>
    <MonthlyPriceChart
      :coinId="id"
    ></MonthlyPriceChart>

  
  <h3>Market Data</h3>
  <table class="table">
    <thead>
      <tr>
        <th scope="col">Price ($USD)</th>
        <th scope="col">% Change (24 hours)</th>
        <th scope="col">% Change (7 days)</th>
        <th scope="col">Market Cap</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>{{ price }}</td>
        <td :class="priceChange24hClass">{{ priceChange24h }}</td>
        <td :class="priceChange7dClass">{{ priceChange7d }}</td>
        <td>{{ marketCap }}</td>
      </tr>
    </tbody>
  </table>
</div>
</template>

<script>
import "bootstrap";
import "bootstrap/dist/css/bootstrap.min.css";
import MonthlyPriceChart from '../components/MonthlyPriceChart';
import axios from "axios";

export default {
  name: "Detailed",
  components: {
    MonthlyPriceChart
  },

  data() {
    return {
      id: this.$route.params.id,
      name: "Loading...",
      symbol: "",
      image: "",
      price: "",
      priceChange24h: "",
      priceChange7d: "",
      priceChange24hClass: "",
      priceChange7dClass: "",
      marketCap: ""
    };
  },

  mounted() {
    this.getData();
  },
  methods: {
    getData() {
      axios({
        method: "GET",
        url: `https://api.coingecko.com/api/v3/coins/${this.id}`,
      })
        .then((resp) => {
          // Update Coin Data Object
          this.name = resp.data.name;
          this.symbol = resp.data.symbol.toUpperCase();
          this.image = resp.data.image.small;
          this.price = "$" + resp.data.market_data.current_price.usd.toFixed(2);
          this.priceChange24h = resp.data.market_data.price_change_percentage_24h.toFixed(2) + "%";
          this.priceChange7d = resp.data.market_data.price_change_percentage_7d.toFixed(2) + "%";
          this.priceChange24hClass = (resp.data.market_data.price_change_percentage_24h >= 0) ? "pos" : "neg";
          this.priceChange7dClass = (resp.data.market_data.price_change_percentage_7d >= 0) ? "pos" : "neg";
          this.marketCap = "$" + resp.data.market_data.market_cap.usd;
        })
        .catch(() => {
          console.error();
        })
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
img.thumbnail {
  width: 30%;
}
.pos {
  color: green;
}
.neg {
  color: red;
}
</style>
