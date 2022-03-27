<template>
    
  <div class="home">
    <h2>Ville par pays</h2>
   
    <label for="country">Pays:</label>
            <select class="form-control" id="select" @input="fetchCities">
              
              <option disabled value="0">Choisissez un pays</option>
              <option
                v-for="country in data.allCountries"
                :key="country.id"
                :value="country._links.cities.href"
              >
                {{ country.name }}
              </option>
            </select>
            

            <div class="col">
               
               <cityForm v-bind:cities="data.allCities" />
            </div>

  </div> 

</template>

<script setup>

import CityForm from "@/components/CityForm.vue";
import { reactive, onMounted } from "vue";
import CityList from "@/components/CityList.vue";

const data = reactive({
  allCities: [],
  allCountries: [],
});



function fetchCities() {
  // Utilise l'API ad-hoc pour avoir le pays de chaque ville
  fetch(document.getElementById("select").value)
    .then((response) => response.json())
    .then((json) => {
      console.log( json)
      data.allCities= []
      let liste = json._embedded.cities;
      for(let c=0;c<liste.length;c++){
        data.allCities.push(liste[c])
      }
    })
    .catch((error) => console.log());
}

// Utilise l'API REST auto-générée pour avoir les pays
function fetchCountries() {
  fetch("api/countries")
    .then((response) => response.json())
    .then((json) => {
      data.allCountries = json._embedded.countries;
    })
    .catch((error) => console.log());
}

onMounted(() => {
  fetchCities(); // Récupération des villes
  fetchCountries(); // Récupération des pays 
});
</script>