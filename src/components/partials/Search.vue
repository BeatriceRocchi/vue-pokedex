<script>
export default {
  props: {
    btnText: String,
  },
  data() {
    return {
      pokemonToSearch: "",
      userPokemons: [],
      isClicked: false,
      toUpdate: false,
    };
  },

  methods: {
    passPokemonToSearch() {
      this.isClicked = true;
      this.$emit("passPokemonToSearch", this.pokemonToSearch, false);
    },

    catchPokemon() {
      this.$emit("catchPokemon");
    },
  },
};
</script>

<template>
  <div class="container">
    <div id="lights-box">
      <div class="light big"></div>
      <div class="light red"></div>
      <div class="light yellow"></div>
      <div class="light green"></div>
    </div>
    <div id="search-box">
      <div>
        <input type="text" v-model="pokemonToSearch" />
        <button @click="passPokemonToSearch">Find</button>
      </div>

      <button v-if="btnText !== ''" @click="catchPokemon()">
        {{ btnText }}
      </button>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: space-between;
  width: 100% !important;

  #lights-box {
    display: flex;

    .light {
      border: 1px solid rgba(black, 0.3);
      box-shadow: inset -4px -4px rgba(black, 0.3);
      height: 20px;
      width: 20px;
      border-radius: 50%;
      margin-right: 10px;

      &.big {
        height: 70px;
        width: 70px;
        box-shadow: inset -6px -6px rgba(black, 0.3);
        background-color: #13eaf7;
        border: 5px solid white;
      }

      &.red {
        background-color: #d80e1e;
      }

      &.yellow {
        background-color: #e0e759;
      }

      &.green {
        background-color: #a3f858;
      }
    }
  }

  #search-box {
    // display: flex;
    // justify-content: space-between;
  }
}
</style>
