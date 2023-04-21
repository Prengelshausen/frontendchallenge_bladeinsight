<template>
  <div id="app">
    <div class="flex container justify-between items-center mx-auto m-8">
        <input type="text" v-model="search" class="border rounded-md shadow py-2 pl-4 pr-24" placeholder="Search for a country ...">
        <select @change="filterByRegion" class="border rounded-md shadow p-2">
          <option value="">Filter by Region</option>
          <option v-for="(region, index) in regions" :key="index" :value="region">
            {{ region }}
          </option>
        </select>
    </div>
    <div class="flex flex-wrap justify-center container mx-auto w-full">
      <CountryCard :filtered="filtered" :countries="countries"/>
    </div>
  </div>
</template>

<script>

import axios from "axios";
import CountryCard from '@/components/CountryCard.vue';
export default {
  name: 'App',
  components: {
    CountryCard,
  },
  data() {
    return {
      search: "",
      filtered: [],
      countries: [],
      regions: ["Africa","Americas","Antarctic","Asia","Europe","Oceania"],
    }
  },
  watch: {
    // watches input field for changes
    search() {
      // filters every common country name that matches with the v-model of the input field
      this.filtered = this.countries.filter(country => {
        return country.name.common.toLowerCase().includes(this.search.toLowerCase());
      })
    }
  },
  methods: {
    async getAllCountries() {
      let res = await axios.get("https://restcountries.com/v3.1/all");
      this.countries = res.data;
      this.filtered = [...this.countries];
      console.log(this.countries);
    },
    filterByRegion(e) {
      const selectedRegion = e.target.value;
      if( selectedRegion.length === 0 ) {
        this.filtered = this.countries;
      } else {
        this.filtered = this.countries.filter(country => country.region == selectedRegion);
      }
    }
    
  },
  created () {
    //as soon as the page loaded, the API Call is made with the getAllCountries method
    this.getAllCountries();
  },
}
</script>

<style>
.custom-shadow {
  box-shadow: 0px 4px 25px rgba(9, 9, 12, 0.1);
}
</style>