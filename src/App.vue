<template>
</template>

<script>
import axios from "axios"


export default {
  name: 'App',
  data() {
    pokemons: [];
  },
  components: {
  },
  created: async function() {
    try {
      const req = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0");
      this.pokemons = req.data.results;
      console.log("Pokemons", this.pokemons);
    } catch (error) {
      console.error("error", error);
    }
  }
}
</script>

<style>
</style>
