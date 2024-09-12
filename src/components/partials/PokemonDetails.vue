<script>
export default {
  props: {
    pokemonData: JSON,
  },
};
</script>

<template>
  <!-- Upper part: image -->
  <div class="img-container">
    <div class="img-box">
      <img
        v-if="pokemonData.name"
        :src="pokemonData.sprites.front_default"
        :alt="pokemonData.name"
      />
    </div>
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
      <div class="progress-bar">
        <div
          class="progress-bar-full"
          :style="{ width: statItem.base_stat + 'px' }"
        >
          {{ statItem.base_stat }}
        </div>
      </div>
    </li>
  </ul>
</template>

<style lang="scss" scoped>
@import "../../assets/scss/partials/variables";
.img-container {
  clip-path: polygon(0 0, 80% 0, 100% 20%, 100% 100%, 0 100%);
  background-color: #e3e3e3;
  height: 220px;
  width: 60%;
  margin: 20px auto;
  padding: 20px;
  border-radius: 10px;
  border-left: 8px solid darken(#e3e3e3, 30%);
  border-bottom: 8px solid darken(#e3e3e3, 30%);

  .img-box {
    background-color: white;
    margin-top: 30px;
    height: 80%;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
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
    border: 1px solid white;
    width: 200px;

    &-full {
      background-color: $color-primary;
    }
  }
}
</style>
