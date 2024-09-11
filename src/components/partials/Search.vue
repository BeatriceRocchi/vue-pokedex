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
  <div id="search-box">
    <div>
      <input type="text" v-model="pokemonToSearch" />
      <button @click="passPokemonToSearch">Find</button>
    </div>

    <button v-if="isClicked" @click="catchPokemon()">
      {{ btnText }}
    </button>
  </div>
</template>

<style lang="scss" scoped>
#search-box {
  display: flex;
  justify-content: space-between;
}
</style>
