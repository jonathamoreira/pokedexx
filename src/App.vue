<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter ">
      <img src="./assets/pokedex.png" alt="logo">
      <hr>
      <input type="text" placeholder="buscar pokemon" v-model="busca" class="input is-rounded">
      <button class="button is-success is-focused" id="buscabtn" @click="buscar">Buscar</button>
      <div v-for="(poke, index) in filterdPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon-list.vue';

export default {
  name: 'App',
  data(){
    return{
      pokemons: [],
      filterdPokemons: [],
      busca:''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res =>{
      this.pokemons = res.data.results;
      this.filterdPokemons = res.data.results;
    })
  },
  components: {
    Pokemon
  },
  methods: {
    buscar: function(){
      this.filterdPokemons = this.pokemons;
      if(this.busca == '' || this.busca == " "){
        this.filterdPokemons = this.pokemons;
      }else{
        this.filterdPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  }
  /*
  computed: { //Resultado em tempo real, não precisa do buttun
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == " "){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  }*/
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
#buscabtn{
  margin-top: 2%;
}
</style>
