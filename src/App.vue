<template>
  <h1>{{ name }}</h1>
  <div>Pokemon ID: <input type="number" v-model="pokeID" /></div>
  <button @click="setPokemonID(-1)" :disabled="pokeID <= 1">Previous</button>
  <button @click="setPokemonID(1)" :disabled="pokeID <= -1">Next</button>
  <div v-for="image in images" :key="image">
    <img :src="image" />
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      pokeID: 1,
      name: "",
      sprites: {},
    };
  },
  methods: {
     fetchPokemon: function (id) {
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
        this.name = response.data.name;
        this.sprites = response.data.sprites;
      });
    },
    setPokemonID: function (num) {
      this.pokeID = this.pokeID + num;
    },
  },
  computed: {
    images: function () {
      const newData = Object.keys(this.sprites)
        .map((prop) => {
          return this.sprites[prop];
        })
        .filter(function (img) {
          return typeof img === "string";
        });
      console.log(newData);
      return newData;
    },
  },
  created() {
    this.fetchPokemon(this.pokeID);
  },
  watch: {
    pokeID: function (pokeID) {
      // console.log(`pokeID changed to ${pokeID}`);
      this.fetchPokemon(pokeID);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>