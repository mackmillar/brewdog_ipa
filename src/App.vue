<template lang="html">
  <div id="app">
    <h1>Brewdog Beers</h1>
    <beer-list :beerList="beerList"></beer-list>
    <beer-details :beer="selectedBeer"></beer-details>
    <div v-if="favourites.length > 0">
      <h2>Favourites</h2>
      <ul>
        <li v-for="favourite in favourites">{{favourite.name}} <button v-on:click="removeFromFavourites(favourite)">Remove</button></li>
      </ul>
    </div>
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
      favourites: [],
      page2: [],
      newList: []
    }
  },
  mounted(){
    this.getBeers()
    
    eventBus.$on('beer-selected', (beer) => {
      this.selectedBeer = beer
    })
    eventBus.$on('favourite', (beer) =>{
      this.favourites.push(beer)
    })
    eventBus.$emit('favouitesList', (this.favourites))
  },
  methods: {
    removeFromFavourites(favourite){
      const newList = this.favourites.filter(beer => favourite != beer)
      this.favourites = newList
    },
    getBeers: function () {
      const page1 = fetch('https://api.punkapi.com/v2/beers?page=1&per_page=80')
      .then(res => res.json())
      .then(beerList => this.beerList = beerList)

      const page2 = fetch('https://api.punkapi.com/v2/beers?page=2&per_page=80')
      .then(res => res.json())
      .then(page2 => this.page2 = page2)

      Promise.all([page1, page2])
      .then(this.beerList.forEach(beer => this.newList.push(beer)))
    },
    
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
