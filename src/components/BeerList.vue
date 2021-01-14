<template>
  <div>
    <select v-on:change="handleSelect" v-model="selectedBeer">
        <option disabled value="" placeholder>Select Beer</option>
        <option v-for="(beer, index) in beerList" :value="beer" :key="index">{{beer.name}}</option>
    </select>
  </div>
</template>

<script>
import { eventBus } from '../main.js'
import ListItem from './ListItem.vue'

export default {
    name: 'beer-list',
    props: ['beerList'],
    data(){
        return{
            'selectedBeer': {},
             
        }
    },
    methods:{
        handleSelect() {
            eventBus.$emit('beer-selected', this.selectedBeer)
        }
    }
}
</script>

<style>

</style>