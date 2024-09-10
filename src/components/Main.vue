<script>
import Search from "./partials/Search.vue";
import PokemonDetails from "./partials/PokemonDetails.vue";

export default {
  data() {
    return {
      pokemonData: {},
    };
  },
  components: {
    Search,
    PokemonDetails,
  },
  methods: {
    handlePokemonDetails(data) {
      this.pokemonData = data;
      console.log(this.pokemonData);
    },
  },
};
</script>

<template>
  <main id="pokedex">
    <!-- Left side: pokemon searchbar, pokemon catch/release and pokemon info -->
    <div>
      <Search @sendPokemonDetails="handlePokemonDetails" />
      <PokemonDetails :pokemonData="pokemonData" />
    </div>
    <!-- Right side: my pokemon list -->
    <div>RIGHT</div>
  </main>
</template>

<style lang="scss" scoped>
@import "../assets/scss/partials/variables";
#pokedex {
  background-color: $color-pokedex;
  margin: 100px auto;
  height: calc(100vh - 300px);
  width: 60%;
  display: flex;

  div {
    width: 50%;
    padding: 10px;
  }
}
</style>
