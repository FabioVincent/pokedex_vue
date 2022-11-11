<template>
   
  <header class="header">
  

    <div class=" text">
      <h2>Press on the Pokéball for random Pokémon</h2> 
    </div> 
    <label for="">
     
      <button
      class="searchButton"
      @click="searchPokemon"
      >
      
    </button>
    </label>
  </header>


  <main 
  class="main"
  v-if="Object.entries(pokemonData).length>0"
  >

    <section class="pokemonCard">
     
      <div class="pokemonImage">

        <h1>Pokémon</h1>
        <img :src="pokemonData.sprites.front_default" :alt="pokemonData.name" width="250" height="250">
        <h2 class="pokemonName">{{pokemonData.name}}</h2>
      </div>


      <div class="description">
      <ul class="type">
        <h3>Type:</h3>
        <li
        v-for="(type, index) in pokemonData.types"
        :key="index"
        :class="type.type.name"
        >
        <span>{{type.type.name}}</span>
        </li>

      </ul>

      <ul class="stats">
        <h3>Stats:</h3>
        <li
        v-for="(stat, index) in pokemonData.stats"
        :key="index"
        >
          <span>{{stat.stat.name}} -> {{stat.base_stat}} </span>
        </li>
      </ul>
    </div>
    </section>
  </main>
</template>

<script>

import axios from "axios";



export default{
  name: "App",

  data(){
    return{
      pokemonData: {},
      pokemonID: '',
    }
  },

 methods: {
  randomPoke() {
      const max = 900;
      const min = 1;
      this.pokemonID = Math.floor(Math.random() * (max - min + 1)) + min;
    },
async searchPokemon(){
  this.randomPoke();
  try {
    const pokemon = await axios
          .get(`https://pokeapi.co/api/v2/pokemon/${this.pokemonID}`)
          .then(function (response) {
            return response.data;
          });

        this.pokemonData = pokemon;
        console.log(pokemon);
        return pokemon;
  } catch (error) {
    alert('Pokemon was not found')
  }
}
  }
}
</script>


<style lang="scss">


  @import './style.css';




</style>
