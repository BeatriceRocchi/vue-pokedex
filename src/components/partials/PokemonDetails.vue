<script>
export default {
  props: {
    pokemonData: JSON,
  },
};
</script>

<template>
  <!-- Upper part: image -->
  <div class="img-box">
    <img
      v-if="pokemonData.name"
      :src="pokemonData.sprites.front_default"
      :alt="pokemonData.name"
    />
  </div>
  <!-- Lower part: details -->
  <ul>
    <li>Name: {{ pokemonData.name }}</li>
    <li>Type: {{ pokemonData.types[0].type.name }}</li>
    <li>Height: {{ pokemonData.height }} ''</li>
    <li>Weight: {{ pokemonData.weight }} lbs</li>
  </ul>
</template>

<style lang="scss" scoped>
.img-box {
  background-color: white;
  width: 50%;
  height: 150px;
  margin: 30px auto;
  display: flex;
  align-items: center;
  justify-content: center;
}

ul {
  list-style: none;
}
</style>
