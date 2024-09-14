<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';


const panels = ref([]);
const loading = ref(true);
const error = ref(null);
const powerProduction = ref(null);
const monthEnergy = ref(null);
const todayEnergy = ref(null);
const leftTimeEnergy = ref(null);
const logs = ref([]);

const fetchSolarPanelData = async () => {
  try {
    const response = await axios.get('https://raw.githubusercontent.com/ott-fogliata/vuejs-s2i-repository/master/solar-panels.json');
    powerProduction.value = response.data['power-production'];
    monthEnergy.value = response.data['month-energy'];
    todayEnergy.value = response.data['today-energy'];
    leftTimeEnergy.value = response.data['left-time-energy'];
    logs.value = response.data.logs;

    panels.value = response.data;
  } catch (err) {
    error.value = err;
  } finally {
    loading.value = false;
  }
};


onMounted(() => {
  fetchSolarPanelData();
});


</script>

<template>

<p class="title">Hotel Pomelia</p>

<div class="container" id="weather-div">
  <!-- Template di weatherAPI -->
  
    <!-- Slot per weatherAPI -->
    <slot></slot>

  </div>
 

<!-- Controllo stato di caricamento -->
<div v-if="loading">
  <p>Loading solar panel data...</p>
</div>
<div v-else>

  <!-- PRIMA RIGA CON DATI PANELLI SOLARI -->
  <div class="container">
    <div class="row">
      <!-- Energy prod -->
      <div class="col-md-6">
        <div class="dates">
          <h5>Power Production</h5>
          <h1>{{ powerProduction }} kW</h1>
        </div>
      </div>
      <!-- Month energy -->
      <div class="col-md-6">
        <div class="dates">
          <h5>This month's Energy</h5>
          <h1>{{ monthEnergy }} kWh</h1>
        </div>
      </div>
    </div>
  </div>

  <!-- SECONDA RIGA CON DATI PANNELLI SOLARI -->
  <div class="container">
    <div class="row">
      <div class="col-md-6">
        <div class="dates">
          <h5>Today's Energy</h5>
          <h1>{{ todayEnergy }} kWh</h1>
        </div>
      </div>
      <div class="col-md-6">
        <div class="dates">
          <h5>Left Time Energy</h5>
          <h1>{{ leftTimeEnergy }} kWh</h1>
        </div>
      </div>
    </div>
  </div>

  <!-- DESCRIZIONE LOGS -->
  <div class="container-fluid justify-content-center" id="logs-dates">
    <h2>LOGS:</h2>
    <div v-if="logs.length >= 6">
      <div v-for="(log, index) in logs.slice(0, 6)" :key="index" class="row-logs">
        <p>{{ log.date }} - {{ log.type }} - {{ log.text }}</p>
      </div>
    </div>
    <div v-else>
      <p>No logs available or loading logs...</p>
    </div>
  </div>

  <!-- Gestione degli errori -->
  <div v-if="error">
    <p>Error loading data: {{ error.message }}</p>
  </div>

</div>
  </template> 

<style>

.title{
font-family: "Great Vibes", cursive;
font-weight: 600;
font-style: normal;
font-size: 45px;
margin-top: 20px;
margin-left: 20px;

}
.dates{
  border-radius: 10px;
  background-image: linear-gradient(to right top, #faee0c, #fee12d, #fed541, #fcc950, #f7bf5e);
  padding: 10px 20px;
  margin-top: 10px;
 
  
}

#weather-div{
  border-radius: 10px;
  background-color:#97d2de;
  padding: 20px 20px;
  margin-bottom: 20px;
  width:auto;
  display: flex;
  height:auto; 
  
  
    
}

#logs-dates{
    font-size: 18px;
    margin-top: 20px;
    padding: 20px 15px;
    
}

.row-logs{
background-color: #bab7c2;
padding: 8px;
margin-top: 10px;
border-radius: 15px;
display: flex;


}
/*MEDIA DISPLAY*/
@media (max-width: 480px) {
.title{
  font-size: 38px;
  display: flex;
  justify-content: center;
}
#weather-div{
        margin-top:20px;
        margin-right: 20px;
        margin-left: 20px;
        width:auto;
        
    }
}

</style>