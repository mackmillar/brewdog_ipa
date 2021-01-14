<template>
  <div id="app">
    <h1>Brewdog Beers</h1>
    <beer-list :beerList="beerList"></beer-list>
    <beer-details :beer="selectedBeer"></beer-details>
  </div>
</template>

<script>
import { eventBus } from './main.js'
import BeerList from './components/BeerList.vue'
import BeerDetails from './components/BeerDetails.vue'

export default {
  name: 'App',
  components: {
    "beer-list": BeerList,
    "beer-details": BeerDetails
  },
  data(){
    return {
      beerList: [],
      selectedBeer: null,
      favourites: []
    }
  },
  mounted(){
    fetch('https://api.punkapi.com/v2/beers')
    .then(res => res.json())
    .then(beerList => this.beerList = beerList)

    eventBus.$on('beer-selected', (beer) => {
      this.selectedBeer = beer
    })
    eventBus.$on('favourite', (beer) =>{
      this.favourites.push(beer)
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
</style>
