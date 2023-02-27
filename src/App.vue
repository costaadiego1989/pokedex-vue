<template>
  <div class="pokedex">
    <div v-for="(pokemon, index) in pokemons" :key="pokemon.url">
      <PokeList :number="index" :url="pokemon.url" :name="pokemon.name" class="poke-content" />
    </div>
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

<style scoped>
.pokedex {
  max-width: 960px;
  display: flex;
  flex-wrap: wrap;
  gap: 0.75rem;
  align-items: center;
  justify-content: center;
  margin: 2rem auto;
}

.poke-content {
  padding: 0.75rem;
  border: 1px solid black;
  border-radius: 0.5rem;
  display: flex;
  flex-direction: column;
  width: 182px;
  box-shadow: 4px 4px 16px -3px rgba(0,0,0,0.5);
  transition: .5s;
}

.poke-content:hover {
  translate: 0px -0.75rem;
  cursor: pointer;
}
</style>
