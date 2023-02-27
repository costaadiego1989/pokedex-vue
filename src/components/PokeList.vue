<template>
  <div>
    <div class="imageContainer">
      <img
        :src="currentImage"
        alt=""
        @mouseenter="changeAvatar()"
        @mouseleave="changeAvatar()"
      />
    </div>
    <span>{{ number + 1 }}.</span>
    <span class="heart">
      <img
        src="../assets/love.png"
        width="24"
        alt=""
        @click="addToFavorites(pokemon)"
      />
    </span>
    <h1>{{ uppercase(name) }}</h1>
    <div class="aditional-info">
      <small>Peso: {{ pokemon.weight }}</small>
      <small>Tipo: {{ pokemon.type }}</small>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "PokeList",
  data() {
    return {
      currentImage: "",
      isFront: true,
      pokemon: {
        front: "",
        name: "",
        back: "",
        weight: "",
        type: "",
      },
    };
  },
  created: async function () {
    try {
      const req = await axios.get(this.url);

      this.pokemon.front = req.data.sprites.front_default;
      this.pokemon.back = req.data.sprites.back_default;
      this.pokemon.weight = req.data.weight;
      this.pokemon.name = req.data.name;
      this.pokemon.type = req.data.types[0].type.name;
      this.currentImage = this.pokemon.front;

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
  methods: {
    changeAvatar() {
      if (this.isFront) {
        this.isFront = !this.isFront;
        this.currentImage = this.pokemon.back;
      } else {
        this.isFront = !this.isFront;
        this.currentImage = this.pokemon.front;
      }
    },
    addToFavorites(pokemon) {
      let items = JSON.parse(
        localStorage.getItem("pokemonsFavourites") || "[]"
      );
      items = [...items, pokemon];
      localStorage.setItem("pokemonsFavourites", JSON.stringify(items));
    },
  },
};
</script>

<style scoped>
h1 {
  font-family: "Helvetica";
  font-size: 1.4rem;
  color: #393836;
  text-align: center;
}

span {
  font-size: 0.75rem;
  position: absolute;
}

.aditional-info {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
}

.imageContainer {
  display: flex;
  justify-content: center;
}

.heart {
  position: relative;
  left: 8rem;
  top: -5.85rem;
}
</style>
