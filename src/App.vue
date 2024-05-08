<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/pokedex.png">
      <input type="text" class="input is-rounded" placeholder="Buscar pokémon pelo nome" v-model="busca"> 
      <!-- <button id="btnBusca" class="button is-fullwidth is-success">Buscar</button> -->
      <div v-for="(poke, index) in resultadoBusca" :key="index">
        <PokemonComponent :num="index + 1" :name="poke.name" :url="poke.url"/> 
      </div>
    </div>
    <div>
      <input type="text" name="nome" v-model="nome" >
      <hr>
      <h2> {{ nome }}</h2>
    </div>
  </div>
</template>

<script>

import axios from "axios";
import PokemonComponent from "./components/PokemonComponent";

export default {
  name: 'App',
  methods: {
    getPokemon: function() { 
      axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res =>{
        this.pokemon = res.data.results;
      });
    }
  },
  beforeMount() {
    this.getPokemon()
  },
  data(){
    return {
      pokemon: [],
      nome: "Walter Bahia",
      busca: ''
    }
  },
  components:{
    PokemonComponent
  },
  computed: {
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' '){
        return this.pokemon;
      }else{
        return this.pokemon.filter(p => p.name == this.busca);
      }
    }
  }
}
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
#btnBusca {
  margin-top: 2%;
}
</style>
