<script setup>
import { ref, reactive, computed, onMounted } from "vue"

  const datePicker = ref('');

  const _seconds = 1000;

  const _minutes = _seconds * 60

  const _hours = _minutes * 60

  const _days = _hours * 24

  const end = ref(new Date(2024, 5, 30, 0, 4, 10))

  const now = ref(new Date());

  const distance = computed(() => end.value.getTime() - now.value.getTime());

  const days = computed(() => formatNum(Math.floor(distance.value / _days)));
  
  const hours = computed(() => formatNum(Math.floor((distance.value % _days) / _hours)));

  const minutes = computed(() => formatNum(Math.floor((distance.value % _hours) / _minutes)));

  const seconds = computed(() => formatNum(Math.floor((distance.value % _minutes) / _seconds)));

  function formatNum(num) {
    return num < 10 ? "0" + num : num;
  }

  onMounted(() => {
    const timer = setInterval(() => {
      now.value = new Date();

      if(distance.value < 0) {
        clearInterval(timer);
        return;
      }      
    }, 1000);
  });
  
</script>

<template>
  <h1>Countdown Timer</h1>

  <label for="start">Pick a date</label>

  <input
    v-model="datePicker"
    type="date"
    name="trip-start"
    value="2024-05-07"
    min="2024-05-07"
    max="3000-12-31" />

    <p> The date is {{datePicker}}</p>

  <div class="grid-timer">
    <div>
      <span class="timer-count">{{ days }}</span>
      <span>days</span>
    </div>
    <div>
      <span class="timer-count">{{ hours }}</span>
      <span>hours</span>
    </div>
    <div>
      <span class="timer-count">{{ minutes }}</span>
      <span>minutes</span>
    </div>
    <div>
      <span class="timer-count">{{ seconds }}</span>
      <span>seconds</span>
    </div>
  </div>

</template>

<style scoped>

@media (min-width:776px) {
  .grid-timer {
    grid-template-columns: repeat(4, 1fr);
    margin-top: 3rem
  }
}

span {
  display: block;
  font-size: .875rem;
  text-transform: uppercase;
}

.timer-count {
  font-size: 2.5rem;
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
