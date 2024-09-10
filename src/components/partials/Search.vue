<script>
import axios from "axios";
export default {
  data() {
    return {
      pokemonToSearch: "",
      apiUrl: "https://pokeapi.co/api/v2/pokemon/",
    };
  },
  methods: {
    searchPokemon() {
      console.log(this.pokemonToSearch);
      axios
        .get(this.apiUrl + this.pokemonToSearch)
        .then((result) => {
          console.log(result.data.name);
          console.log(result.data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<template>
  <input type="text" v-model="pokemonToSearch" />
  <button @click="searchPokemon">Find</button>
</template>

<style lang="scss" scoped></style>
