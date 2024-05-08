<script setup>
import { ref, reactive, computed, onMounted } from "vue"

  const time = reactive({
    displayDays: 0,
    displayHours: 0,
    displayMinutes: 0,
    displaySeconds: 0
  });

  const datePicker = ref('');

  const _seconds = 1000;

  const _minutes = _seconds * 60

  const _hours = _minutes * 60

  const _days = _hours * 24

  const distance = ref(0);

  const days = computed(() => Math.floor(distance.value / _days));
  
  const hours = computed(() => Math.floor((distance.value % _days) / _hours));

  const minutes = computed(() => Math.floor((distance.value % _hours) / _minutes));

  const seconds = computed(() => Math.floor((distance.value % _minutes) / _seconds));

  function formatNum(num) {
    return num < 10 ? "0" + num : num;
  }

  onMounted(() => {
    const timer = setInterval(() => {
      const now = new Date();
      const end = new Date(2024, 4, 10, 10, 50, 10)
      distance.value = end.getTime() - now.getTime();

      if(distance.value < 0) {
        clearInterval(timer);
        return;
      }

      time.displayMinutes = formatNum(minutes.value);
      time.displaySeconds = formatNum(seconds.value);
      time.displayHours = formatNum(hours.value);
      time.displayDays = formatNum(days.value);
      
    }, 1000);
  });
  
</script>

<template>
  <h1>Countdown Timer</h1>

  <label for="start">Pick a date</label>

  <input
    v-model="datePicker"
    type="date"
    id="start"
    name="trip-start"
    value="2024-05-07"
    min="2024-05-07"
    max="3000-12-31" />

    <p> The date is {{datePicker}}</p>

  <div class="grid-timer">
    <div>
      <span class="timer-count">{{ time.displayDays }}</span>
      <span>days</span>
    </div>
    <div>
      <span class="timer-count">{{ time.displayHours }}</span>
      <span>hours</span>
    </div>
    <div>
      <span class="timer-count">{{ time.displayMinutes }}</span>
      <span>minutes</span>
    </div>
    <div>
      <span class="timer-count">{{ time.displaySeconds }}</span>
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
