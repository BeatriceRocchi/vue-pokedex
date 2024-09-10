<script>
import axios from "axios";
export default {
  data() {
    return {
      pokemonToSearch: "",
      pokemonData: {},
      userPokemons: [],
      apiUrl: "https://pokeapi.co/api/v2/pokemon/",
    };
  },
  methods: {
    searchPokemon() {
      axios
        .get(this.apiUrl + this.pokemonToSearch)
        .then((result) => {
          this.pokemonData = result.data;
          this.$emit("sendPokemonDetails", this.pokemonData);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    catchPokemon() {
      // To mantain the pokemon of a previous localStorage
      if (localStorage.userPokemons) {
        this.userPokemons = localStorage.userPokemons.split(",");
      }
      this.userPokemons.push(this.pokemonData.name);
      localStorage.setItem("userPokemons", this.userPokemons);
    },
  },
};
</script>

<template>
  <div id="search-box">
    <div>
      <input type="text" v-model="pokemonToSearch" />
      <button @click="searchPokemon">Find</button>
    </div>
    <button @click="catchPokemon">Catch</button>
  </div>
</template>

<style lang="scss" scoped>
#search-box {
  display: flex;
  justify-content: space-between;
}
</style>
