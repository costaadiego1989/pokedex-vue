<template>
  <div class="header">
    <h2>Pokedéx</h2>
    <div class="searchContainer">
      <input
        type="text"
        placeholder="Buscar Pokémon pelo nome..."
        v-model="search"
      />
      <button @click="filteredPokemon" v-show="filteredPokemon.length <= 0">Buscar</button>
      <button @click="filteredPokemon" v-show="filteredPokemon.length > 0">Limpar</button>
    </div>
  </div>

  <div class="pokedex">
    <div
      v-for="(pokemon, index) in filteredPokemons.length > 0
        ? filteredPokemons
        : pokemons"
      :key="pokemon.url"
    >
      <PokeList
        :number="index"
        :url="pokemon.url"
        :name="pokemon.name"
        class="poke-content"
      />
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
      filteredPokemons: [],
      search: "",
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
  methods: {
    filteredPokemon() {
      if (this.search !== "" || this.search !== " ") {
        this.filteredPokemons = this.pokemons;
      }
      const filter = this.pokemons.filter(
        (pokemon) => pokemon.name === this.search
      );
      this.filteredPokemons = filter;
      this.search = "";
    },
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
  border: 1px solid #393836;
  border-radius: 0.5rem;
  display: flex;
  flex-direction: column;
  width: 182px;
  box-shadow: 4px 4px 16px -3px rgba(0, 0, 0, 0.5);
  transition: 0.5s;
}

.poke-content:hover {
  translate: 0px -0.75rem;
  cursor: pointer;
}

h2 {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 3rem;
  color: #393836;
}

.header {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.searchContainer {
  display: flex;
  align-items: center;
  gap: 1rem;
}

button {
  width: 10rem;
  border-radius: 30px;
  background: #393836;
  color: #e6e6e6;
  height: 2.65rem;
  box-sizing: border-box;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.5s;
}

button:hover {
  border: 1px solid #393836;
  background-color: transparent;
  color: #393836;
}

input[type="text"] {
  font-family: Helvetica, sans-serif;
  width: 40rem;
  height: 2.5rem;
  padding: 1rem;
  font-size: 1rem;
  border-radius: 20px;
  border: 1px solid #393836;
}
</style>
