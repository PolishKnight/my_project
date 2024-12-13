<script setup>
import { ref } from 'vue';
import { my_project_backend } from 'declarations/my_project_backend/index';
let input = ref('');

const getDataFromNBP =  async () => {
  const res = await fetch("https://api.nbp.pl/api/exchangerates/tables/A/?format=json");
  const jsonData = await res.json();
  const rates = jsonData[0].rates;
  console.log(rates);
}
getDataFromNBP();

</script>

<template>
  <main>
    <img src="/logo2.svg" alt="DFINITY logo" />
    <br />
    <br />
  </main>
</template>
