<script setup>

</script>

<template>
  <h1>Countdown Timer</h1>

  <label for="start">Pick a date</label>

  <input type="date" id="start" name="trip-start" value="2024-05-07" min="2018-01-01" max="3000-12-31" />

  <div class="grid-timer">
    <div>
      <p>0 :</p>
      <p>days</p>
    </div>
    <div>
      <p>0 :</p>
      <p>hours</p>
    </div>
    <div>
      <p>0 :</p>
      <p>minutes</p>
    </div>
    <div>
      <p>0</p>
      <p>seconds</p>
    </div>
  </div>

</template>

<style scoped>

@media (min-width:776px) {
  .grid-timer {
    grid-template-columns: repeat(4, 1fr);
  }
}
.grid-timer {
  display: grid;
}

label {
  display: block;
  font:
    1.5rem 'Fira Sans',
    sans-serif;
}

input,
label {
  margin: 0.4rem 0;
}

input {
  font-size: 1.2rem;
}

</style>
