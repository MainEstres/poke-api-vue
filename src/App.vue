<script setup>
import { onMounted, ref, computed} from 'vue';
import axios from 'axios';
import CardPoke from './components/CardPoke.vue';

const pokemonData = ref([])
const numPoke = ref()





const getData = async () => {
  try {
    if(isNaN(numPoke) && numPoke.value > 150){
      alert("Por favor, ingresa un número válido mayor que 0 y menor a 150");
      return;
    }

    const { data } = await axios.get(`https://pokeapi.co/api/v2/pokemon?limit=${numPoke.value}`);
    const pokemones = [];
    for (const { url } of data.results) {
      const { data } = await axios.get(url);
      pokemones.push({
        name: data.name,
        image: data.sprites.other.dream_world.front_default,
        descubierto: false
      });
    }
    pokemonData.value = pokemones;
    console.log( numPoke.value);

  } catch (error) {
    console.error(error);
  }

}

const comprobar = (name) => {
  const foundPokemon = pokemonData.value.find(
    (pokemon) => pokemon.name.toLowerCase() === name
  );
  if(foundPokemon){
    foundPokemon.descubierto = true
  }
  

}
const counter = computed(() => {
    return pokemonData.value.filter((pokemon) => pokemon.descubierto).length;
  });

</script>

<template>
  <div class="container">
    <div class="d-flex justify-content-center row text-center ">
      <div>
        <img src="/International_Pokémon_logo.svg.png" alt="pokemon" class="w-50" />
      </div>
      <h1>¿Quién es este pokemon?</h1>
      <p>Pokemones encontrados: <span class="text-success">{{counter}}</span></p>
      <div>
        <h4>¿Cuantos Pokemons quieres cargar?</h4>
        <input type="number" v-model="numPoke">
        <button @click=getData()>Cargar</button>
      </div>
      <CardPoke v-for="(poke, i) in pokemonData" :key="i" :name="poke.name" :image="poke.image"
        :descubierto="poke.descubierto" @send="comprobar" />
    </div>
  </div>

</template>

<style scoped></style>