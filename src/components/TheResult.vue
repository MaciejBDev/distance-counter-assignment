<template>
  <div class="results">
    <h1>Distance</h1>
    <hr />
    <h2>{{ resultDistancekm }} km</h2>
    <h2>{{ resultDistancekm * 1000 }} m</h2>
  </div>
</template>

<script>
export default {
  props: {
    resultDistancekm: Number,
  },
};
</script>

<style scoped>
.results {
  margin: 2rem auto;
  max-width: 40rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 2rem;
  background-color: #ffffff;
}
</style>