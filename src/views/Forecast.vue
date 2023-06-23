<template>
  <div>
    <h2 class="text-center mb-3 text-xl">Current Weather</h2>
    <div class="mx-auto w-1/3 bg-weather-secondary rounded shadow-lg p-4 text-white font-bold transition hover:scale-105">
      <!-- Display current forecast data here -->
      <div class="text-center">{{ currentWeather.name }}</div>
      <img :src="getWeatherIconUrl(currentWeather.weather[0].icon)" alt="Weather Icon" class="mx-auto my-4">
      <div class="text-center">Temp: {{ currentWeather.main.temp }}</div>
      <div class="text-center">Humidity: {{ currentWeather.main.humidity }}</div>
      <div class="text-center">Wind Speed: {{ currentWeather.wind.speed }}</div>
    </div>

    <h2 class="text-center mt-8 mb-5 text-xl">5-Day Forecast</h2>
    <div class="flex flex-wrap justify-center text-white font-bold ">
      <!-- Display 5-day forecast cards here -->
      <div v-for="forecast in forecastData" :key="forecast.date" class="mx-4 my-6 bg-blue rounded shadow-lg p-6 bg-weather-secondary transition hover:scale-105 duration-300">
        <div class="text-center">{{ forecast.date }}</div>
        <img :src="getWeatherIconUrl(forecast.weatherIcon)" alt="Weather Icon" class="mx-auto my-4">
        <div class="text-center">Temp: {{ forecast.temp }}</div>
        <div class="text-center">Humidity: {{ forecast.humidity }}</div>
        <div class="text-center">Wind Speed: {{ forecast.windSpeed }}</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    currentWeather: Object,
    forecastData: Array,
  },
  methods: {
    getWeatherIconUrl(iconCode) {
      return `https://openweathermap.org/img/w/${iconCode}.png`;
    },
  },
};
</script>
