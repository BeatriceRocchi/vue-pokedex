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
  <!-- TODO: Add stats and style -->
  <ul v-if="pokemonData.name">
    <li>Name: {{ pokemonData.name }}</li>
    <li>Type: {{ pokemonData.types[0].type.name }}</li>
    <li>Height: {{ pokemonData.height }} ''</li>
    <li>Weight: {{ pokemonData.weight }} lbs</li>
  </ul>

  <h4>Stats:</h4>
  <ul>
    <li class="stat-item" v-for="(statItem, id) in pokemonData.stats" :key="id">
      <p>{{ statItem.stat.name }}</p>
      <div class="progress-bar" :style="{ width: statItem.base_stat + 'px' }">
        {{ statItem.base_stat }}
      </div>
    </li>
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

.stat-item {
  p {
    display: inline-block;
    width: 50%;
    min-width: fit-content;
  }

  .progress-bar {
    display: inline-block;
    background-color: black;
  }
}
</style>
