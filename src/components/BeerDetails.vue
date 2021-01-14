<template>
  <div v-if="beer">
      <h2>{{beer.name}}</h2>
      <p>{{beer.description}}</p>
      <img :src="beer.image_url" alt="">
      <button v-on:click="addToFavourites">Favourite</button>
  </div>
</template>

<script>
import { eventBus } from '../main.js'

export default {
    name: 'beer-details',
    props: ['beer'],
    methods: {
        addToFavourites(){
            eventBus.$emit("favourite", this.beer)
        }
    }
}
</script>

<style>
img {
    height: 250px;
}
</style>