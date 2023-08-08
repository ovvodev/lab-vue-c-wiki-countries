<script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const country = ref({});

const fetchCountry = async () => {
  const id = route.params.alpha3Code;
  const response = await fetch(
    `https://ih-countries-api.herokuapp.com/countries/${id}`
  );
  const data = await response.json();
  country.value = data;
};

onMounted(() => {
  fetchCountry();
});
</script>

<template>
  <p>Country View{{ country }}{{ route.params }}</p>
</template>

<style scoped>
p {
  margin: 30px 50px;
}
</style>
