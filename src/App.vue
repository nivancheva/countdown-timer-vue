<script setup>
import { ref, reactive, computed, onMounted } from "vue"

  const options = ref([
  { text: 'Greenwich Mean Time (UTC+0)', value: '0' },
  { text: 'Eastern European Time (UTC+2)', value: '2' },
  { text: 'Maldives Time (UTC+5)', value: '5' },
  { text: 'India Time Zone (UTC+9)', value: '9' },
  { text: 'Brasília Summer Time (UTC-2)', value: '-2' },
  { text: 'Alaska Standard Time (UTC-9)', value: '-9' },
])

  const _seconds = 1000;

  const _minutes = _seconds * 60

  const _hours = _minutes * 60

  const _days = _hours * 24

  const days = computed(() => formatNum(Math.floor(distance.value / _days)));
  
  const hours = computed(() => formatNum(Math.floor((distance.value % _days) / _hours)));

  const minutes = computed(() => formatNum(Math.floor((distance.value % _hours) / _minutes)));

  const seconds = computed(() => formatNum(Math.floor((distance.value % _minutes) / _seconds)));

  const tomorrow = new Date(new Date().getTime() + _days);

  const endDate = ref(formatDay(tomorrow));
  
  const end = computed(() => new Date(endDate.value));

  const now = ref(new Date());

  const distance = computed(() => end.value - now.value);

  const selectedTimezone = ref(0);

  function formatNum(num) {
    return num < 10 ? "0" + num : num;
  }

  function formatDay (date) {  
    if (!(date instanceof Date)) {
      throw new Error('Invalid "date" argument. You must pass a date instance')
    }

    const year = date.getFullYear()
    const month = formatNum(String(date.getMonth() + 1))
    const day = formatNum(String(date.getDate()))

    return `${year}-${month}-${day}`
  }

  function updateLocalStorage() {
    localStorage.setItem('_dt',endDate.value);
  }

  onMounted(() => {
    const localStorageDate = localStorage.getItem('_dt');

    if(localStorageDate) {
      endDate.value = localStorageDate;
    }

    const timer = setInterval(() => {
      now.value = new Date(new Date().getTime() + selectedTimezone.value * _hours);
    }, 1000);
  });
  
</script>

<template>
  <h1>Countdown Timer</h1>

  <label for="start">Pick a date</label>
  <input
    v-model="endDate"
    id="start" 
    @change="updateLocalStorage"
    type="date"
    name="trip-start"
    :min="formatDay(tomorrow)"
    max="3000-12-31" />

  <label for="timezone">Select Timezone</label>
  <select v-model="selectedTimezone" name="timezone" id="timezone">
    <option v-for="(option, i) in options" :value="option.value" :key="i">
      {{ option.text }}
    </option>

  </select>

  <div v-if="distance > 0" class="grid-timer">
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
label{
  margin: 0.4rem 0;
}

input,
select {
  font-size: 1.2rem;
}

select {
  padding: .25rem;
}

</style>
