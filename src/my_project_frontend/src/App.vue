<script setup>
import { ref } from 'vue';
import { my_project_backend } from 'declarations/my_project_backend/index';

const rates = ref([]);
const iloscWaluty = ref([])

const getDataFromNBP =  async () => {
  const res = await fetch("https://api.nbp.pl/api/exchangerates/tables/A/?format=json");
  const jsonData = await res.json();
  rates.value = jsonData[0].rates;
  // console.log(rates.value);
  iloscWaluty.value = jsonData[0].rates.map(() => 0);
}

const kupWalute = async (index) => {
  console.log(rates.value[index]);
  const ilosc = BigInt(iloscWaluty.value);
  const cena = BigInt(rates.value[index].mid * 10e16);
  console.log(ilosc, cena);

  const koszt = await my_project_backend.calculate_currency_price(ilosc, cena);
  console.log(koszt / BigInt(10e16));
}

getDataFromNBP();

</script>

<template>
  <main>
    <img src="/logo2.svg" alt="DFINITY logo" />
    <br />
    <br />

    <table>
      <tr>
        <th>Nazwa waluty</th>
        <th>Kod waluty</th>
        <th>Cena</th>
        <th>Ilość waluty</th>
        <th></th>
      </tr>
      <tr v-for="(rate, index) in rates">
        <td>{{ rate.currency }}</td>
        <td>{{ rate.code }}</td>
        <td>{{ rate.mid }}</td>
        <td><input type="number" v-model="iloscWaluty" /></td>
        <td><button @click="kupWalute(index)">Kup</button></td>
      </tr>
    </table>
  </main>
</template>
