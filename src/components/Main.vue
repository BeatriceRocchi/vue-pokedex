<script>
import axios from "axios";
import Search from "./partials/Search.vue";
import PokemonDetails from "./partials/PokemonDetails.vue";

export default {
  data() {
    return {
      pokemonData: {},
      userPokemons: [],
      pokemonToSearch: "",
      apiUrl: "https://pokeapi.co/api/v2/pokemon/",
      btnText: "",
      // isCatched: false,
    };
  },
  components: {
    Search,
    PokemonDetails,
  },

  methods: {
    searchPokemon(pokemon, isCatched) {
      this.pokemonToSearch = pokemon;
      axios
        .get(this.apiUrl + this.pokemonToSearch)
        .then((result) => {
          this.pokemonData = result.data;
        })
        .catch((error) => {
          console.log(error);
        });

      if (isCatched || this.userPokemons.includes(this.pokemonToSearch)) {
        this.btnText = "Free";
      } else {
        this.btnText = "Catch";
      }
    },

    catchPokemon() {
      // To mantain the pokemon of a previous session in the localStorage
      if (localStorage.userPokemons) {
        this.userPokemons = localStorage.userPokemons.split(",");
      }
      if (this.btnText === "Catch") {
        this.userPokemons.push(this.pokemonToSearch);
        localStorage.setItem("userPokemons", this.userPokemons);
        this.btnText = "Free";
      } else {
        const idToDelete = this.userPokemons.indexOf(this.pokemonToSearch);
        this.userPokemons.splice(idToDelete, 1);
        localStorage.setItem("userPokemons", this.userPokemons);
      }
    },
  },
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
      <Search
        :btnText="btnText"
        @passPokemonToSearch="searchPokemon"
        @catchPokemon="catchPokemon"
      />
      <PokemonDetails :pokemonData="pokemonData" />
    </div>

    <!-- Right side: my pokemon list -->
    <div>
      <h3>My pokemons</h3>
      <ul>
        <li v-for="(pokemon, id) in userPokemons" :key="id">
          <a href="#" @click="searchPokemon(pokemon, true)">{{ pokemon }}</a>
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
