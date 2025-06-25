<script setup>
import { onMounted, ref, computed} from 'vue';
import axios from 'axios';
import CardPoke from './components/CardPoke.vue';

const pokemonData = ref([])
console.log(pokemonData.value)




const getData = async () => {
  try {
    const { data } = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=20');
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

onMounted(() => {
  getData();
})
</script>

<template>
  <div class="container">
    <div class="d-flex justify-content-center row text-center ">
      <div>
        <img src="/International_Pokémon_logo.svg.png" alt="pokemon" class="w-50" />
      </div>
      <h1>¿Quién es este pokemon?</h1>
      <p>Pokemones encontrados: <span class="text-success">{{counter}}</span></p>
      <CardPoke v-for="(poke, i) in pokemonData" :key="i" :name="poke.name" :image="poke.image"
        :descubierto="poke.descubierto" @send="comprobar" />
    </div>
  </div>

</template>

<style scoped></style>