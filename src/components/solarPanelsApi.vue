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


const fetchSolarPanelData = async () => {
  try {
    const response = await axios.get('https://raw.githubusercontent.com/ott-fogliata/vuejs-s2i-repository/master/solar-panels.json');
    powerProduction.value = response.data['power-production'];
    monthEnergy.value = response.data['month-energy'];
    todayEnergy.value = response.data['today-energy'];
    leftTimeEnergy.value = response.data['left-time-energy'];

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

<div class="container">
      <div class="row">
        <!---template di weatherAPI-->
        <div class="col-md-4" >
            <div class="dates2">
<!--slot per weatherapi-->
                <slot></slot>
</div>
</div> 
     
      
     <!----energy prod-->
   <div class="col-md-4" >
      <div class="dates2">
        <h3>Energy Production</h3>
        <p v-if="loading">Caricamento...</p>
        <p v-else>{{ powerProduction }} kW</p>
    
      </div>
    </div>
    <!---month energy-->
    <div class="col-md-4">
      <div class="dates2">
        <h3>This Month's Energy</h3>
        <p v-if="loading">Caricamento...</p>
        <p v-else>{{ monthEnergy }} kWh</p>
      </div>
    </div>
  
       
        
       
      
  

<!--2 riga-->
<div class="container" id="container">
  <div class="row" id="riga">
    <div class="col-md-4">
      <div class="dates2" >
        <h3>today Energy</h3>
        <p v-if="loading">Caricamento...</p>
        <p v-else>{{ todayEnergy }} kWh</p>
        
      </div>
    </div>
    <div class="col-md-4">
      <div class="dates2">
        <h3>Left time energy</h3>
        <p v-if="loading">Caricamento...</p>
        <p v-else>{{ leftTimeEnergy }} kWh</p>
        
      </div>
    </div>
    
  </div>
</div>

</div>
</div>






</template>

<style>
.dates2{
  border-radius: 10px;
  background: #a1f480;
  padding: 10px 20px;
  margin-top: 10px;
}



</style>