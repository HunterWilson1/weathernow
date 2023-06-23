<template>
    <header class="sticky top-0 bg-weather-primary shadow-lg z-50">
      <nav class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6">
        <RouterLink :to="{ name: 'home' }">
          <div class="flex items-center gap-3 flex-1">
            <i class="fa-solid fa-sun text-2xl"></i>
            <p class="text-2xl">WeatherNow</p>
          </div>
        </RouterLink>
      </nav>
    </header>
  
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        type="text"
        v-model="searchQuery"
        placeholder="Search for a city"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow"
      />
      <button @click="searchWeather" class="px-4 py-2 mt-4 bg-weather-secondary text-white rounded">Search</button>
    </div>
    <WeatherView :currentWeather="currentWeather" :forecastData="forecastData" v-if="currentWeather && forecastData" />
  </main>
</template>

<script>

import WeatherView from "./Forecast.vue";

export default {
  components: {
    WeatherView,
  },
  data() {
    return {
      searchQuery: "",
      currentWeather: null,
      forecastData: null,
      APIKey: "59e0d5247e1028ae9dbf1071b7d55e24",
    };
  },
  methods: {
    async getWeatherData(city) {
      const currentWeather = await this.getCurrentWeather(city);
      const forecastData = await this.getFiveDayForecast(city);
      this.currentWeather = currentWeather;
      this.forecastData = forecastData;
    },
    getCurrentWeather(city) {
      const queryURL = `https://api.openweathermap.org/data/2.5/weather?q=${city}&units=imperial&appid=${this.APIKey}`;

      return fetch(queryURL)
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          return data;
        });
    },
    getFiveDayForecast(city) {
  const queryURL = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=imperial&appid=${this.APIKey}`;

  return fetch(queryURL)
    .then((response) => response.json())
    .then((data) => {
      console.log(data);

      const forecasts = [];
      for (let i = 0; i < data.list.length; i += 8) {
        const item = data.list[i];
        forecasts.push({
          date: item.dt_txt,
          weatherIcon: item.weather[0].icon,
          temp: item.main.temp,
          humidity: item.main.humidity,
          windSpeed: item.wind.speed,
        });
        console.log(item);
      }

      return forecasts;
    });
},

    searchWeather() {
      this.getWeatherData(this.searchQuery);
    },
  },
};
</script>
