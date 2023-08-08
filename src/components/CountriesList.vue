<script setup>
import { ref, onMounted } from "vue";
import { RouterLink } from "vue-router";


const countriesList = ref([]);

const selectedCountry = ref({});

const borderCountries = ref({});

const fetchCountries = async () => {
  const response = await fetch(
    "https://ih-countries-api.herokuapp.com/countries"
  );
  const data = await response.json();
  countriesList.value = data;
};
const getFlagUrl = (alpha2Code) => {
  return `http://flagpedia.net/data/flags/icon/72x54/${alpha2Code.toLowerCase()}.png`;
};

const selectCountry = (country) => {
  selectedCountry.value = country;
};

const getBorderUrl = async (value) => {
  const response = await fetch(`https://ih-countries-api.herokuapp.com/countries/${value}`);
  const borderCountryData = await response.json();
  borderCountries.value = borderCountryData
};

onMounted(() => {
  fetchCountries();
});
</script>
<template>
  <div class="main-container">
    <ul>
      <li
        class="country-card"
        v-for="country in countriesList"
        @click="selectCountry(country)"
        
      >
        <img :src="getFlagUrl(country.alpha2Code)" />
        <p>{{ country.name.common }}</p>
      </li>
    </ul>
    <section v-if="selectedCountry.name" >
      <img :src="getFlagUrl(selectedCountry.alpha2Code)" alt="" />
      <h2>{{ selectedCountry.name.common }}</h2>
      <p>Capital {{ selectedCountry.capital }}</p>
      <p>Area {{ selectedCountry.area }}km²</p>
      <p>Borders 
        <ul >
           <!-- <li v-for="borderCountry in selectedCountry.borders" :key="borderCountry.borders">
    <p>{{ borderCountry }}</p><RouterLink :to="{ name : 'country'}">more</RouterLink></li> -->
    <li v-for="borderCountry in selectedCountry.borders" :key="borderCountry">
    <RouterLink :to="{ name: 'countryDetails', params: { alpha3Code: borderCountry } }">
      {{ borderCountry }}
    </RouterLink>
  </li>
        </ul>
        </p>
    </section>
  </div>
</template>

<style scoped>
.main-container {
  display: flex;
  justify-content: space-between;
}

section {
  width: 50%;
  margin: 30px auto;
  position: fixed;
  top: 100px;
  right: -50px;
  align-content: center;
  text-align: center;
}
.country-card {
  text-decoration: none;
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  flex-direction: column;
  align-content: center;
  width: 400px;
  border: solid black 1px;
  padding: 20px;
  margin: 0 5px;
}

li {
  align-items: center;
  text-align: center;
}
img {
  width: 80px;
}

section img {
  width: 150px;
}
</style>
