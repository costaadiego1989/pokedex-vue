<template>
  <div>
    <img src="{{ pokemon.front }}" alt="">
    <h1>{{ number + 1 }}. {{ uppercase(name) }}</h1>
    <small>{{ pokemon.weight }}</small>
    <small>{{ pokemon.type }}</small>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "PokeList",
  data() {
    return {
      pokemon: {},
    };
  },
  created: async function () {
    try {
      const req = await axios.get(this.url);

      this.pokemon.front = req.data.sprites.front_default;
      this.pokemon.back = req.data.sprites.back_default;
      this.pokemon.weight = req.data.weight;
      this.pokemon.type = req.data.types[0].type.name;

    } catch (error) {
      console.error("error", error);
    }
  },
  props: {
    number: Number,
    url: String,
    name: String,
  },
  computed: {
    uppercase() {
      return (v) => {
        return v[0].toUpperCase() + v.slice(1);
      };
    },
  },
};
</script>

<style>
</style>
