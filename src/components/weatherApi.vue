<script setup>
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';
import solarPanelsApi from './solarPanelsApi.vue'


const weather = ref(null);
const loading = ref(true);
const localtime = ref(null);
const apiKey = 'fa35bb3711c94fda97f140914240309';
const location = 'Roma';


const iconUrl = computed(() => {
  return weather.value ? `https:${weather.value.condition.icon}` : '';
});


const fetchWeather = async () => {
  const url = `http://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${location}&aqi=no`;

  try {
    const response = await axios.get(url);
    weather.value = response.data.current;
    const fullLocalTime = response.data.location.localtime; 
    localtime.value = fullLocalTime.split(' ')[1]; 
  } catch (error) {
    console.error("Errore nel caricamento dei dati meteo:", error);
  } finally {
    loading.value = false;
  }
};


onMounted(fetchWeather);




</script>


<template>
<!----tutto il template in solarpanelsApi-->
<solarPanelsApi>

<p>weather roma</p> 
    <p> {{ localtime }}</p>
     <div>
<div v-if="loading">
  <p>Loading...</p>
</div>
<div v-else-if="weather">
  <p>Temperatura attuale: {{ weather.temp_c }}°C</p>
  <p>Condizioni: {{ weather.condition.text }}</p>
  <div>
    <img :src="iconUrl" :alt="weather.condition.text" />
  </div>
</div>
<div v-else>
  <p>Error loading data.</p>
</div>
</div>

</solarPanelsApi>


</template>

