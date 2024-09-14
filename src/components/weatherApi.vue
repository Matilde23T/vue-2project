<script setup>
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';
import solarPanelsApi from './solarPanelsApi.vue';

const weather = ref(null);
const loading = ref(true);
const localtime = ref(null);
const apiKey = 'fa35bb3711c94fda97f140914240309';
const location = 'Rome';


const iconUrl = computed(() => {
  return weather.value ? `https:${weather.value.condition.icon}` : '';
});

//API METEO 
const fetchWeather = async () => {
  const url = `http://api.weatherapi.com/v1/current.json?key=${apiKey}&q=${location}&aqi=no`;

  try {
    const response = await axios.get(url);
    weather.value = response.data.current;
    // ORARIO 
    const fullLocalTime = response.data.location.localtime; 
    localtime.value = fullLocalTime.split(' ')[1]; 
  } catch (error) {
    console.error("Errore caricamento dati meteo:", error);
  } finally {
    loading.value = false;
  }
};

onMounted(fetchWeather);


// Stato contatore
const count = ref(0);
const digits = computed(() => {
  return count.value.toString().padStart(9, '000002234').split(''); 
});
const incrementCounter = () => {
  count.value += 1;
};
//ogni sec
setInterval(incrementCounter, 1000); 

</script>

<template>
    <solarPanelsApi>
    
  <div class="container-fluid">

  <!-- Riga dati e icona meteo -->
    <div class="row d-flex align-items-center">
    <!-- Colonna dati meteo -->
    <div class="col d-flex justify-content-between align-items-center" id="datimeteo-contenitore">
          <div id="colonnadatimeteo">
            <h1>Rome</h1>
            <p class="localtime-date">{{ localtime }}</p>

            <div v-if="loading">
              <p>Loading...</p>
            </div>

            <div v-else-if="weather">
              <p class="dates-api">{{ weather.temp_c }}°C</p>
            </div>

            <div v-else>
              <p>Error loading data.</p>
            </div>
          </div>

          <!-- Icona meteo -->
          <div class="weather-icon" v-if="weather">
            <img :src="iconUrl" :alt="weather.condition.text" class="imgicon"/>
          </div>
        </div>
      </div>

      <!--contatore-->
      <div class="row mt-4">
        <div class="col-md-5">
          <div class="counter-container d-flex justify-content-start">
            <div class="digit-box" v-for="(digit, index) in digits" :key="index">
              {{ digit }}
            </div>
          </div>
        </div>
      </div>
    </div>

  </solarPanelsApi>
</template>

<style scoped>
.title-dates{
    font-size: 22px;
}
.localtime-date{
    font-size: 30px;
    font-weight: lighter;
}
.dates-api{
    font-size: 35px;
    font-weight: bold;
}
#datimeteo-contenitore {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
#colonnadatimeteo {
    flex: 1;
    padding-right: 20px;
}

.weather-icon img {
    height: 250px;
    width: 250px;
}

.counter-container {
    padding: 10px;
    border-radius: 10px;
}

.digit-box {
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    border: 1px solid grey;
    border-radius: 5px;
    font-size: 20px;
    font-weight: bold;
    background-color: whitesmoke;
}

/*MEDIA DISPLAY*/
@media (max-width: 480px) {

    #datimeteo-contenitore {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
    }

    .weather-icon img {
        height: 80px;
        width: 80px;
        margin-bottom: 52px;
    }

    #colonnadatimeteo {
        font-size: 14px;
    }
    .dates-api{
      font-size: 25px;
      margin-left: 3px;
    }
}

</style>