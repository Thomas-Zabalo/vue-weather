<template>
  <!-- Conteneur principal du composant -->
  <div class="weather-container">
    <!-- Titre de l'application -->
    <h1>Weather App</h1>
    
    <!-- Formulaire pour entrer le nom de la ville et récupérer les données météo -->
    <form @submit.prevent="fetchWeather">
      <!-- Champ de saisie pour entrer le nom de la ville -->
      <input v-model="city" placeholder="Enter city name" required />
      <!-- Bouton pour soumettre le formulaire et récupérer les données météo -->
      <button type="submit">Get Weather</button>
    </form>

    <!-- Affiche les informations météo uniquement si `weatherData` est défini -->
    <div v-if="weatherData">
      <!-- Nom de la ville -->
      <h2>{{ weatherData.name }}</h2>
      <!-- Température actuelle -->
      <p>Temperature: {{ weatherData.main.temp }}°C</p>
      <!-- Description du temps -->
      <p>Weather: {{ weatherData.weather[0].description }}</p>
      <!-- Icône météo, URL générée dynamiquement -->
      <img :src="weatherIconUrl" alt="Weather Icon" />
    </div>

    <!-- Affiche un message d'erreur uniquement si `error` est défini -->
    <div v-if="error" class="error">{{ error }}</div>
  </div>
</template>

<script>
// Importation de la bibliothèque axios pour effectuer des requêtes HTTP
import axios from 'axios';

export default {
  // Nom du composant
  name: 'WeatherComponent',
  
  // Données réactives du composant
  data() {
    return {
      // Nom de la ville pour la recherche
      city: 'London',
      // Données météo récupérées
      weatherData: null,
      // Message d'erreur éventuel
      error: null,
    };
  },
  
  // Propriétés calculées
  computed: {
    // URL de l'icône météo, construite à partir du code d'icône fourni par l'API
    weatherIconUrl() {
      if (this.weatherData) {
        // Obtient le code d'icône météo
        const iconCode = this.weatherData.weather[0].icon;
        // Retourne l'URL complète de l'icône
        return `http://openweathermap.org/img/wn/${iconCode}@2x.png`;
      }
      return '';
    },
  },
  
  // Méthodes du composant
  methods: {
    // Méthode asynchrone pour récupérer les données météo
    async fetchWeather() {
      // Clé API pour accéder aux données de l'API OpenWeatherMap
      const apiKey = 'YOUR_API_KEY'; // Remplacez par votre clé API
      // URL de la requête API avec le nom de la ville et la clé API
      const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${apiKey}&units=metric`;

      try {
        // Envoie la requête GET à l'API
        const response = await axios.get(url);
        // Met à jour les données météo avec la réponse de l'API
        this.weatherData = response.data;
        // Réinitialise le message d'erreur
        this.error = null;
      } catch (err) {
        // Met à jour le message d'erreur en cas d'échec de la requête
        this.error = 'Failed to fetch weather data.';
        // Réinitialise les données météo
        this.weatherData = null;
      }
    },
  },
  
  // Hook de cycle de vie appelé lorsque le composant est monté
  mounted() {
    // Récupère les données météo lorsque le composant est monté
    this.fetchWeather(); 
  },
};
</script>

<style scoped>
/* Styles pour le conteneur principal du composant */
.weather-container {
  text-align: center;
  margin: 50px auto;
  max-width: 400px;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

/* Styles pour le champ de saisie */
input {
  padding: 10px;
  margin-right: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

/* Styles pour le bouton */
button {
  padding: 10px 15px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

/* Styles pour le bouton au survol */
button:hover {
  background-color: #0056b3;
}

/* Styles pour les messages d'erreur */
.error {
  color: red;
}

/* Styles pour l'image de l'icône météo */
img {
  margin-top: 10px;
  width: 100px; /* Ajustez la taille selon vos besoins */
  height: auto;
}
</style>
