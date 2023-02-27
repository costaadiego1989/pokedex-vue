<template>
  <div v-for="(pokemon, index) in pokemons" :key="index">
    <PokeList
      :number="index"
      :url="pokemon.url"
      :name="pokemon.name"
    />
  </div>
</template>

<script>
import axios from "axios";

import PokeList from "./components/PokeList.vue";

export default {
  name: "App",
  data() {
    return {
      pokemons: [],
    };
  },
  components: {
    PokeList,
  },
  created: async function () {
    try {
      const req = await axios.get(
        "https://pokeapi.co/api/v2/pokemon?limit=151&offset=0"
      );
      this.pokemons = req.data.results;
    } catch (error) {
      console.error("error", error);
    }
  },
};
</script>

<style>
</style>
