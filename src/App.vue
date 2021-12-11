<template>
  <div class="container p-5">
    <h1 class="text-center mb-4">Coin Market</h1>

    <input 
      type="text"
      class="form-control bg-dark text-light rounded-2 border-0 my-4"
      placeholder="Search Coin"
      @keyup="searchCoin"
      v-model="textSearch"
    >

    <table class="table table-dark my-2">
      <thead>
        <tr>
          <th v-for="title in titles" :key="title">{{ title }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(coin, index) in filteredCoins" :key="index + 1">
          <td>{{ index + 1 }}</td>
          <td>
            <img :src="coin.image" width="25" :alt="coin.name">
            <span class="ms-2">{{ coin.name }}</span>
            <span class="ms-2 text-uppercase text-muted">{{ coin.symbol }}</span>
          </td>
          <td>${{ coin.current_price }}</td>
          <td 
            :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']"
          >
            {{ coin.price_change_percentage_24h }}%
          </td>
          <td>${{ coin.total_volume.toLocaleString() }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      coins: [],
      titles: [
        '#', 'Coin', 'Price', 'Price Change (%)', 'Volume 24h'
      ],
      textSearch: "",
      filteredCoins: []
    }
  },
  async mounted(){
    const res = await fetch("https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false")
    const data = await res.json()
    console.log(data)
    this.coins = data
    this.filteredCoins = data
  },
  methods:{
    searchCoin(){
      this.filteredCoins = this.coins.filter(
        (coin) => coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) |
            coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      )

    }
  }
}

</script>

<style>

</style>
