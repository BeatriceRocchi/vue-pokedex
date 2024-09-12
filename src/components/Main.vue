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
    <div id="left-side">
      <Search
        :btnText="btnText"
        @passPokemonToSearch="searchPokemon"
        @catchPokemon="catchPokemon"
      />
      <PokemonDetails :pokemonData="pokemonData" />
    </div>

    <!-- Right side: my pokemon list -->
    <div id="right-side">
      <h3>My pokemons</h3>
      <ul>
        <li v-for="(pokemon, id) in userPokemons" :key="id">
          <a
            href="#"
            @click="searchPokemon(pokemon, true), (pokemonToSearch = pokemon)"
            >{{ pokemon }}</a
          >
        </li>
      </ul>
    </div>
  </main>
</template>

<style lang="scss" scoped>
@import "../assets/scss/partials/variables";
#pokedex {
  margin: 10px auto 20px;
  width: 90%;
  min-height: calc(100vh - 60px - 180px);
  max-width: 1200px;
  display: flex;
  border-radius: 30px;
  border-left: 15px solid darken($color-pokedex, 20%);
  border-bottom: 15px solid darken($color-pokedex, 20%);

  div {
    background-color: $color-pokedex;
    width: 50%;
    padding: 10px;
  }

  #left-side {
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
    border-bottom-left-radius: 10px;
    border-right: 8px solid darken($color-pokedex, 20%);
  }

  #right-side {
    margin-top: 110px;
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px;
  }
}
</style>
