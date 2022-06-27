<template>
  <div class="population">
    <h1 class="population__title">World Population</h1>
    <div id="map" class="population__map"></div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { onMounted } from 'vue';


onMounted(async () => {
  const res = await axios.get('https://countriesnow.space/api/v0.1/countries/population')
  const countries = res.data.data
  const countriesData = [['Country', 'Popularity']]
  countries.forEach(item => {
    if (item.country !== 'World' && item.country !== 'IDA & IBRD total'
      && item.country !== 'Arab World'
      && item.country !== 'Low & middle income'
      && item.country !== 'East Asia & Pacific'
      && item.country !== 'IBRD only'
      && item.country !== 'East Asia & Pacific (excluding high income)'
      && item.country !== 'Late-demographic dividend'
      && item.country !== 'Lower middle income'
      && item.country !== 'Middle income'
      && item.country !== 'Central Europe and the Baltics'
      && item.country !== 'East Asia & Pacific (excluding high income)'
      && item.country !== 'Late-demographic dividend'
      && item.country !== 'South Asia'
      && item.country !== 'High income'
      && item.country !== 'OECD members'
      && item.country !== 'South Asia (IDA & IBRD)'
      && item.country !== 'IDA total'
      && item.country !== 'South Asia'
      && item.country !== 'East Asia & Pacific (IDA & IBRD countries)'
      && item.country !== 'Upper middle income'
      && item.country !== 'Early-demographic dividend') {
      let country = item.country
      let population = item.populationCounts[item.populationCounts.length - 1].value
      countriesData.push([country, population])
    }
  })
  google.charts.load('current', {
    'packages': ['geochart'],
  });
  google.charts.setOnLoadCallback(drawRegionsMap);

  function drawRegionsMap() {
    var data = google.visualization.arrayToDataTable(countriesData);

    const chart = new google.visualization.GeoChart(document.getElementById('map'));

    chart.draw(data, {});

  }
})
</script>


<style lang="scss">
@import './assets/base.css';
.population {
  background-color: #fff;
  text-align: center;
  color: #000;
  &__title {
    margin-bottom: 20px;
  }
  &__map {
    margin: 0 auto;
    max-width: 900px;
    height: 500px;
  }
}
</style>
