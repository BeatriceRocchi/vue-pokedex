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
      pokemonActive: "",
      idPokemonActive: -1,
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

    changePokemonActive(isBefore) {
      if (isBefore) {
        if (this.idPokemonActive === 0) {
          this.pokemonActive = this.userPokemons[this.userPokemons.length - 1];
          this.idPokemonActive = this.userPokemons.length - 1;
        } else {
          this.pokemonActive = this.userPokemons[this.idPokemonActive - 1];
          this.idPokemonActive = this.idPokemonActive - 1;
        }
      } else {
        if (this.idPokemonActive === this.userPokemons.length - 1) {
          this.pokemonActive = this.userPokemons[0];
          this.idPokemonActive = 0;
        } else {
          this.pokemonActive = this.userPokemons[this.idPokemonActive + 1];
          this.idPokemonActive = this.idPokemonActive + 1;
        }
      }
      this.searchPokemon(this.pokemonActive, true);
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
        this.btnText = "Catch";
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
      <div class="user-pokemons-container">
        <div class="user-pokemons-screen">
          <h3 class="bold-text">My pokemons</h3>
          <ul>
            <li
              class="pokemon-list-el"
              :class="pokemonActive === pokemon ? 'active' : ''"
              v-for="(pokemon, id) in userPokemons"
              :key="id"
            >
              <span
                @click="
                  searchPokemon(pokemon, true),
                    (pokemonActive = userPokemons[id]),
                    (idPokemonActive = id)
                "
              >
                {{ pokemon }}
              </span>
            </li>
          </ul>
        </div>

        <!-- Cross button with arrows -->
        <div class="grid-container">
          <div class="grid-item"></div>
          <div class="grid-item btn" @click="changePokemonActive(true)">
            <i class="fa-solid fa-caret-up"></i>
          </div>
          <div class="grid-item"></div>
          <div class="grid-item btn">
            <i class="fa-solid fa-caret-left"></i>
          </div>
          <div class="grid-item btn btn-center">&#9675;</div>
          <div class="grid-item btn">
            <i class="fa-solid fa-caret-right"></i>
          </div>
          <div class="grid-item"></div>
          <div class="grid-item btn">
            <i
              class="fa-solid fa-caret-down"
              @click="changePokemonActive(false)"
            ></i>
          </div>
          <div class="grid-item"></div>
        </div>
      </div>
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
    padding: 10px;

    .user-pokemons-container {
      height: 100%;
      width: 100%;
      background-color: darken($color-pokedex, 5%) !important;
      border-radius: 10px;

      .user-pokemons-screen {
        background-color: whitesmoke;
        border: 1px solid darken(#e3e3e3, 30%);
        width: 80%;
        height: 65%;
        margin: 10px auto;
        overflow-y: scroll;

        .pokemon-list-el {
          padding: 2px;

          span {
            padding: 0 10px;
          }

          &:hover {
            background-color: darken(#e3e3e3, 30%);
            cursor: pointer;
          }
        }
      }

      .grid-container {
        display: grid;
        width: 150px;
        margin: 0 auto;
        grid-template-columns: 1fr 1fr 1fr;
        padding: 0;

        .grid-item {
          width: 100%;
          aspect-ratio: 1/1;
          color: white;
          display: flex;
          justify-content: center;
          align-items: center;
          background-color: darken($color-pokedex, 5%);

          &.btn {
            background-color: black;
            border: 1px solid rgb(44, 44, 44);
            font-size: 2.5rem;
            color: rgb(44, 44, 44);
            padding: 0;
          }
        }
      }
    }
  }
}
</style>
