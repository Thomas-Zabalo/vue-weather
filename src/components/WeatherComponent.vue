<template>
    <div class="weather-container">
      <h1>Weather App</h1>
      <form @submit.prevent="fetchWeather">
        <input v-model="city" placeholder="Enter city name" required />
        <button type="submit">Get Weather</button>
      </form>
  
      <div v-if="weatherData">
        <h2>{{ weatherData.name }}</h2>
        <p>Temperature: {{ weatherData.main.temp }}°C</p>
        <p>Weather: {{ weatherData.weather[0].description }}</p>
      </div>
  
      <div v-if="error" class="error">{{ error }}</div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        city: 'London',
        weatherData: null,
        error: null,
      };
    },
    methods: {
      async fetchWeather() {
        const apiKey = '4a27a1e493fbe19db141f206d30b9c26'; 
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${apiKey}&units=metric`;
  
        try {
          const response = await axios.get(url);
          this.weatherData = response.data;
          this.error = null;
        } catch (err) {
          this.error = 'Failed to fetch weather data.';
          this.weatherData = null;
        }
      },
    },
    mounted() {
      this.fetchWeather(); // Fetch weather data when component mounts
    },
  };
  </script>
  
  <style scoped>
  .weather-container {
    text-align: center;
    margin: 50px auto;
    max-width: 400px;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  }
  
  input {
    padding: 10px;
    margin-right: 10px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  button {
    padding: 10px 15px;
    border: none;
    background-color: #007bff;
    color: white;
    border-radius: 5px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  
  .error {
    color: red;
  }
  </style>
  