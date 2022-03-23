<template>
  <h1>POKÉDEX</h1>
  <form @submit.prevent="apiCall">
    <input v-model="name" type="text" required placeholder="pokemon's name" />
    <button>GO!</button>
  </form>
  
  <div v-if="noErrMsg">
    <p>{{ pokemons.name.toUpperCase() }}</p>
    <img :src="pokemons.sprites.front_default" :alt="pokemons.name" width="200" height="200">
    <h3>Type</h3>
      <p><em>{{ pokemons.types[0].type.name }}</em></p>
    <h3>Ability</h3>
      <p><em>{{ pokemons.abilities[0].ability.name }}</em></p>
    <h3>Weight, Height</h3>
      <p><em>{{ pokemons.weight / 10 }} kg, {{ pokemons.height * 10}} cm</em></p>
  </div>

  <div class="pokeimage" v-if="!noErrMsg" > 
    <!-- <img src="public/pokeball.png" alt="pokeballs" width="300" height="140" > -->
    <p>Try adding the name of a pokémon.</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

let name = ref('')
let pokemons = ref([])
let noErrMsg = ref(false)

function apiCall() { axios.get(`https://pokeapi.co/api/v2/pokemon/${name.value.toLowerCase()}`)
      .then(response => {
          pokemons.value = response.data
          noErrMsg.value = true
      })
      .catch(error => {
          console.log(error.response)   
          noErrMsg.value = false
      })
}  
</script>

<style>
button {
  font-family: 'Press Start 2P', cursive;
  height: 25px;
  margin: 10px;
  border-radius: 5px;
  padding: 3px;
  background-color: red;
  color: white;
}

button:hover {
  background-color: white;
  color: red;
}

input {
  text-align: center;
  margin: 5px;
  border-radius: 5px;
  width: 150px;
  height: 25px;
}  

.pokeimage {
  margin-top: 50px;
}
</style>