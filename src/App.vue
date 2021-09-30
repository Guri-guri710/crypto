<template>
  <div class="container">
    <div class="row">
    
    <div  style="width:100%" class="text-center">
        <h1>Cripto monedas en el mundo</h1>
    </div>

     <input 
     v-model="coinSearch" 
     @keyup="searchCoin()" 
     type="text" 
     placeholder="Search coin" 
     class="form-control bg-dark text-light rounded-0 border-0 my-4" 
     >
    
       <table class="table table-dark">
    <thead>
      <tr>
        <th v-for="title of titles" :key="title">
          {{title}}
        </th>
      </tr>
    </thead>
    <tbody >
      <tr v-for="(coin, index) of filteredCoins" :key="coin.id">
        <td class="text-muted">
         <span >{{index}} </span> 
        </td>
        <td>
          <img :src="coin.image" alt="Imagen coin" style="width:2rem" class="me-5">
         <span>{{coin.name}}</span>
         <span class="ms-2 text-uppercase text-muted">
           {{coin.symbol}}
         </span>
        </td>
        <td>
          ${{coin.current_price}}
        </td>
        <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success': 'text-danger']">
          {{coin.price_change_percentage_24h}}%
        </td>
        <td>
          {{coin.total_volume.toLocaleString()}}
        </td>
      </tr>
    </tbody>
  </table>
    
    </div>
  </div>


</template>

<script>


export default {
  name: 'App',
  data(){
    return{
      coins:[],
      filteredCoins:[],
      titles:['#','coin', 'price', 'price change', '24h Volumen'],
      coinSearch:'',
    }
  },
  async mounted(){
   const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const data = await res.json()
    console.log(data);
    this.coins = data
    this.filteredCoins = data
  },

  methods:{
    searchCoin(){
     this.filteredCoins = this.coins.filter(coin => coin.name.toLowerCase().includes(this.coinSearch.toLowerCase())
      ||
      coin.symbol.toLowerCase().includes(this.coinSearch.toLowerCase())
     )
    },

  },

}
</script>

<style>
#app {
}
</style>
