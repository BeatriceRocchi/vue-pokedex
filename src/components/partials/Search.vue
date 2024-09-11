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
    catchPokemon(toCatch) {
      // TODO: fix: if a pokemon is included in the userPokemons in the previous session, the button is catch and not free

      // To mantain the pokemon of a previous session in the localStorage
      if (localStorage.userPokemons) {
        this.userPokemons = localStorage.userPokemons.split(",");
      }

      if (toCatch) {
        this.userPokemons.push(this.pokemonData.name);
        localStorage.setItem("userPokemons", this.userPokemons);
      } else {
        const idToDelete = this.userPokemons.indexOf(this.pokemonData.name);
        this.userPokemons.splice(idToDelete, 1);
        localStorage.setItem("userPokemons", this.userPokemons);
      }

      // To pass the update userPokemons list to the parent component
      this.$emit("updatePokemon", this.userPokemons);
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
    <button
      v-if="userPokemons.includes(pokemonToSearch)"
      @click="catchPokemon(false)"
    >
      Free
    </button>
    <button v-else @click="catchPokemon(true)">Catch</button>
  </div>
</template>

<style lang="scss" scoped>
#search-box {
  display: flex;
  justify-content: space-between;
}
</style>
