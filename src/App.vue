<template>
  <div id="app" :class="typeof weather.main != 'undefined' && Math.round(weather.main.temp - 273.15) < 16 ? 'cold': ''">
    <main>
      <div class="search-box">
        <input
          v-model="request"
          class="search-form"
          @keypress="getWeather"
          type="text" placeholder="Search...">
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="weather-location">
          <h1 class="weather-location__header">{{ weather.name }}, {{ weather.sys.country }}</h1>
          <p class="weather-date">{{ setDate() }}</p>
        </div>

        <div class="weather-data">
          <h1 class="weather-temp">{{ Math.round(weather.main.temp - 273.15) }}°c</h1>
          <h2 class="weather-info">{{ weather.weather[0].main }}</h2>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
    weather: {},
    request: '',
  }),
  methods: {
    getWeather(e) {
      if (e.key === 'Enter') {
        fetch(`${process.env.VUE_APP_BASE_URL}weather?q=${this.request}&units-metric&APPID=${process.env.VUE_APP_API_KEY}`)
          .then((res) => res.json())
          .then(this.setData);
      }
    },
    setData(value) {
      this.weather = value;
    },
    setDate() {
      const newDate = new Date();
      const months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      const days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

      const day = days[newDate.getDay()];
      const date = newDate.getDate();
      const month = months[newDate.getMonth()];
      const year = newDate.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style lang="scss">
@import "assets/style/styles";
</style>
