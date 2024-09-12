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
        <input class="input-custom" type="text" v-model="pokemonToSearch" />
        <button class="btn" @click="passPokemonToSearch">Find</button>
      </div>
      <button
        class="btn action"
        :class="btnText === 'Catch' ? 'catch' : 'free'"
        v-if="btnText !== ''"
        @click="catchPokemon()"
      >
        <img src="../../assets/img/pokeball.png" alt="pokeball" />
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
  height: 100px;

  #lights-box {
    display: flex;
  }

  #search-box {
    display: flex;
    flex-direction: column;

    .btn.action {
      margin-top: 10px;
      display: flex;
      align-items: center;
      justify-content: center;

      img {
        height: 20px;
        width: 20px;
        margin-right: 10px;
      }
    }
  }
}
</style>
