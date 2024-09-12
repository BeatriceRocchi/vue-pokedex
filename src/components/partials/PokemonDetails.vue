<script>
export default {
  props: {
    pokemonData: JSON,
  },
};
</script>

<template>
  <!-- Upper part: pokemon image -->
  <div class="img-container">
    <div class="light-container">
      <div class="light red"></div>
      <div class="light red"></div>
    </div>
    <div class="img-box">
      <img
        v-if="pokemonData.name"
        :src="pokemonData.sprites.front_default"
        :alt="pokemonData.name"
      />
    </div>
  </div>

  <!-- Lower part: pokemon details -->
  <div v-if="pokemonData.name" class="details-box">
    <!-- Main pokemon features -->
    <ul>
      <li><span class="bold-text">Name</span>: {{ pokemonData.name }}</li>
      <li>
        <span class="bold-text">Type</span>:
        {{ pokemonData.types[0].type.name }}
      </li>
      <li>
        <span class="bold-text">Height</span>: {{ pokemonData.height }} ''
      </li>
      <li>
        <span class="bold-text">Weight</span>: {{ pokemonData.weight }} lbs
      </li>
    </ul>

    <!-- Statistics -->
    <div class="stats-box">
      <h4 class="bold-text">Stats:</h4>
      <ul>
        <li
          class="stat-item"
          v-for="(statItem, id) in pokemonData.stats"
          :key="id"
        >
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
    </div>
  </div>
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

  .light-container {
    display: flex;
    justify-content: center;
  }

  .img-box {
    background-color: whitesmoke;
    margin-top: 15px;
    height: 80%;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;

    img {
      height: 100%;
    }
  }
}

.details-box {
  margin: 0 10px;
  padding: 10px;
  background-color: whitesmoke;
  border: 1px solid rgba(black, 0.3);
  border-radius: 6px;

  .stats-box {
    margin-top: 10px;

    .stat-item {
      p {
        margin-left: 10px;
        display: inline-block;
        width: 40%;
        min-width: fit-content;
        margin-bottom: 5px;
      }

      .progress-bar {
        display: inline-block;
        background-color: white;
        // border: 1px solid darken(#e3e3e3, 30%);
        width: 200px;

        &-full {
          background-color: #f8e94b;
          color: #f8e94b;
        }
      }
    }
  }
}
</style>
