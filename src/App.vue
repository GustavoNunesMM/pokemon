<template class="body">
  <div id="app" async>
      <input type="text" class="search" v-model="filter"  
      @keypress.enter.prevent="filterPokemons()" placeholder="Pokemon name"> 
   
    <div class="flex">
      <div v-for="(pokemon, pokemonIndex) in pokemonInf" :key="pokemonIndex">
        <Card :pokemon="pokemon"/>
      </div>
      
    </div>
    
  </div>
</template>

<script>
const axios = require("axios")

import Card from './components/Card.vue'

export default {
  data() {
    return{
      filter: "" ,
      pokemonInf: [],
    }
  },

  components: {
    Card
  },

  created() {
    this.getPokemons()
  },

  methods: {
    async getPokemons() {
      const { data: { results } } = await axios.get("https://pokeapi.co/api/v2/pokemon")
      const pokemonsPromisse = results.map(({ url }) => axios.get(url))
      const pokemonsPromisseAll = await Promise.all(pokemonsPromisse)
      this.getPokemonInf(pokemonsPromisseAll)
    },

    async filterPokemons() {
      let inputValue = this.filter.toLowerCase()

      if (inputValue) {
        try {
          const data = await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.filter}`)
          this.getPokemonInf([data])
        } catch(error) {
          this.getPokemons()
        }
        return
      }

      this.getPokemons()
    },

    getPokemonInf(listPokemons) {
      console.log(listPokemons)
      const newListPokemons = listPokemons.map(({ data: { name, height, weight, types, base_experience, sprites } }) => {
        return {
          name, 
          height,
          weight,
          type: types.map(e => e.type.name),
          base_experience,
          firstType: types[0].type.name,
          sprites
        }
      })

      this.pokemonInf = newListPokemons
    },
  }  
}

</script>


<style lang="scss">
body {
  background: linear-gradient(to bottom, #2980b9, #6dd5fa, #ffffff); 
  background-repeat: no-repeat;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000000;
}

.search {
  height: 20px;
  width: 50%;
  margin-right: 0px;
  font-family: inherit;
  border: 0;
  border-bottom: 2px solid black;
  outline: 0;
  font-size: 1.3rem;
  padding: 7px 0;
  background: transparent; 
  
}
::placeholder {
  color:white;
}


button {
  width: 20%;
  height: 20px;
}

.flex {
  padding: 30px;
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  align-content: space-around;
  gap: 40px;
  justify-content: center;

  a {
    font-weight: bold;
    color: #2c3e50;
  }
}
</style>
