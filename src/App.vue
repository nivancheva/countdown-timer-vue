<script setup>
import { ref, reactive, computed, onMounted } from "vue"

  const time = reactive({
    displayDays: 0,
    displayHours: 0,
    displayMinutes: 0,
    displaySeconds: 0
  });

  const _seconds = computed(() => 1000)

  const _minutes = computed(() => _seconds.value * 60)

  const _hours = computed(() => _minutes.value * 60)

  const _days = computed(() => _hours.value * 24)

  onMounted(() => {
    const timer = setInterval(() => {
      const now = new Date();
      const end = new Date(2024, 4, 9, 10, 50, 10)
      const distance = end.getTime() - now.getTime();

      if(distance < 0) {
        clearInterval(timer);
        return;
      }

      const days = Math.floor(distance / _days.value);
      const hours = Math.floor((distance % _days.value) / _hours.value);
      const minutes = Math.floor((distance % _hours.value) / _minutes.value);
      const seconds = Math.floor((distance % _minutes.value) / _seconds.value);
      time.displayMinutes = minutes < 10 ? "0" + minutes : minutes;
      time.displaySeconds = seconds < 10 ? "0" + seconds : seconds;
      time.displayHours = hours < 10 ? "0" + hours : hours;
      time.displayDays = days < 10 ? "0" + days : days;
      
    }, 1000);
  });
  
</script>

<template>
  <h1>Countdown Timer</h1>

  <label for="start">Pick a date</label>

  <input type="date" id="start" name="trip-start" value="2024-05-07" min="2024-05-07" max="3000-12-31" />

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
