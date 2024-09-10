<script>
import Search from "./partials/Search.vue";
import PokemonDetails from "./partials/PokemonDetails.vue";

export default {
  data() {
    return {
      pokemonData: {},
      userPokemons: [],
    };
  },
  components: {
    Search,
    PokemonDetails,
  },
  methods: {
    handlePokemonDetails(data) {
      this.pokemonData = data;
    },
  },
  // computed: {
  //   changeUserPokemons() {
  //     return (this.userPokemons = localStorage.userPokemons);
  //   },
  // },
  mounted() {
    if (localStorage.userPokemons) {
      this.userPokemons = localStorage.userPokemons.split(",");
    }
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
    <div>
      <h3>My pokemons</h3>
      <ul>
        <li v-for="(pokemon, id) in userPokemons" :key="id">
          {{ pokemon }}
        </li>
      </ul>
    </div>
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
