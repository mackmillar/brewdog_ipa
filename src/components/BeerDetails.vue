<template>
  <div v-if="beer">
      <h2>{{beer.name}}</h2>
      <p>{{beer.description}}</p>
      <div>
      <h3>Ingredients</h3>
        <p>Malt</p>
      <ul>
      <li v-for="subIngredient in beer.ingredients.malt">{{subIngredient.name}}</li>
      </ul>
      <p>Hops</p>
      <li v-for="subIngredient in beer.ingredients.hops" v-if="onlyOne(subIngredient, beer.ingredients.hops)">{{subIngredient.name}}</li>
      </ul>
      <p>Yeast</p>
      <li>{{beer.ingredients.yeast}}</li>
      </div>
      <img :src="beer.image_url" alt="">
      <button v-on:click="addToFavourites">Favourite</button>
  </div>
</template>

<script>
import { eventBus } from '../main.js'

export default {
    name: 'beer-details',
    props: ['beer'],
    data(){
    return {
        favouritesList: [],
    }
    },
    methods: {
        addToFavourites(){
            eventBus.$emit("favourite", this.beer),
            this.favouritesList.push(this.beer)
        },
        notFavourited(){
            for (beer in this.favouritesList) {
                if (beer.name === this.beer.name){
                    return false
                }
                return true
            }
        },
        onlyOne(ingredient, array){
            let newList = []
            newList = array.filter(item => item.name === ingredient.name)
            console.log('new list', newList)
            if (newList.length < 2){
                return true
            }
        }
    }
    }
    // mounted(){
    //     eventBus.$on('favouritesList', (favouriteList) =>{
    //         this.favouritesList = favouritesList
    //     })
    // },


</script>

<style>
img {
    height: 250px;
}
</style>