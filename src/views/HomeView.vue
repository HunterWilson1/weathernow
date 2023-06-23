<template>
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
  </main>
</template>

<script setup>
import { ref } from "vue";

const searchQuery = ref("");

const APIKey = "59e0d5247e1028ae9dbf1071b7d55e24";

async function getWeatherData(city) {
  function currentWeather(city) {
    let queryURL =
      "https://api.openweathermap.org/data/2.5/weather?q=" +
      city +
      "&units=imperial&appid=" +
      APIKey;

    return fetch(queryURL)
      .then((Response) => Response.json())
      .then((data) => {
        console.log(data);
        return data;
      });
  }

  function getFiveDayForecast(city) {
    let queryURL =
      "https://api.openweathermap.org/data/2.5/forecast?q=" +
      city +
      "&units=imperial&appid=" +
      APIKey;

    return fetch(queryURL)
      .then((Response) => Response.json())
      .then((data) => {
        console.log(data);

        for (let i = 0; i < data.list.length; i += 8) {
          console.log(data.list[i]);
        }

        return data;
      });
  }

  const cityWeather = await currentWeather(city);
  const forecastData = await getFiveDayForecast(city);

  // Perform any necessary data manipulation or update the Vue component with the retrieved data
  // Example:
  // console.log(cityWeather);
  // console.log(forecastData);
}

function searchWeather() {
  getWeatherData(searchQuery.value);
}
</script>
