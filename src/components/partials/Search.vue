<script>
import axios from "axios";
export default {
  data() {
    return {
      pokemonToSearch: "",
      pokemonData: {},
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
  },
};
</script>

<template>
  <div>
    <input type="text" v-model="pokemonToSearch" />
    <button @click="searchPokemon">Find</button>
  </div>
</template>

<style lang="scss" scoped></style>
