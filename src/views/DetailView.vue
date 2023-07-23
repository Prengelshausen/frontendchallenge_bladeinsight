<template>
  <div>
    <div class="grid lg:grid-cols-2 items-center shadow-md rounded-md m-8 p-12">
      <div class="mb-4 lg:mb-0">
        <img class="border" :src="country.flags.svg" :alt="country.flags.alt">
      </div>
      <div class="flex flex-col">
        <div class="mb-6">
          <p class="font-bold">{{ country.name.official }}</p>
          <p>Native name(s): <i>{{ showOfficialName()}}</i></p>
        </div>
        <div class="flex flex-col justify-between [&>p]:mb-2 ">
          <p>Capital: {{ showCapital() }}</p>
          <p>Region {{ country.region }} in subregion {{ country.subregion || "No subregion specified" }}</p>
          <p>Timezone(s): {{ country.timezones.join(", ") || "No timezone specified"}}</p>
          <p>Area: {{ country.area.toLocaleString("pt-PT") }} km²</p>
          <p>Currency: {{ showCurrencies() }}</p>
          <p>Language(s): {{ showLanguages() }}</p>
          <p v-if="country.borders">Border Countries: 
            <router-link v-for="(bc,index) of country.borders" :key="index" :to="`/detail/${bc}`" class="text-sky-500">
              {{ bc }}, 
            </router-link>
          </p>
          <p v-else>Border Countrie(s): No Border Countries</p>
        </div>
        <a class="w-fit self-center mt-4 font-bold text-sky-500" :href="country.maps.googleMaps" target="_blank" rel="noopener noreferrer">Check out {{ country.name.common }} on Google Maps</a>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"
export default {
  data() {
    return {
      country: {},
    }
  },
  methods: {
    // API Call to get all the information about currently clicked country via Country Code
    async getCountryByCode() {
      let res = await axios.get(`https://restcountries.com/v3.1/alpha/${this.$route.params.cca2}`);
      this.country = res.data[0];
      console.log(this.country);
    },
    showOfficialName() {
      if (!this.country.name.nativeName) return "No official native name specified";
      // get each object key of this.country.name.nativeName as an array
      const nativeNames = Object.keys(this.country.name.nativeName);
      //map through the array to output each individual name of every native language
      const mapNative = nativeNames.map((native) => {
          return this.country.name.nativeName[`${native}`].official;
        }).join(", ");
      return mapNative;
    },
    showCapital() {
      if (!this.country.capital) return "No Capital specified";
      return this.country.capital[0];
    },
    showCurrencies() {
      if (!this.country.currencies) return "No Currencies specified";
      // get each object key of this.country.currencies as an array
      const allCurrencies = Object.keys(this.country.currencies);
      //map through the array to output each individual currency of the country
      const mapCurrencies = allCurrencies.map((currency) => {
          return this.country.currencies[`${currency}`].name;
        }).join(", ");
      return mapCurrencies;
    },
    showLanguages() {
      if (!this.country.languages) return "No Language specified";
      return Object.values(this.country.languages).join(", ");
    },
  },
  created () {
    //as soon as the page loaded, the API Call is made with the getCountryByCode method
    this.getCountryByCode();
  },
}
</script>
