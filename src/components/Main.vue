<script>
import Search from "./partials/Search.vue";

export default {
  components: {
    Search,
  },
};
</script>

<template>
  <main id="pokedex">
    <!-- Left side: pokemon searchbar, pokemon catch/release and pokemon info -->
    <div>
      <Search />
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
  }
}
</style>
