<script setup>
const countriesList = ref([]);

const selectedCountry = ref({});

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
onMounted(() => {
  fetchCountries();
});
</script>

<template></template>

<style></style>
