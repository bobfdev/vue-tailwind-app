<template>
  <!-- v-if is a directive to conditionally render a block -->
  <!-- "!loading" = if it's not loading we'll show our data -->
  <main v-if="!loading">
    <!-- bind in "text" prop to whatever the title is, which is 'Global' per below -->
    <!-- bind in "dataDate" prop to whatever the date is -->
    <DataTitle :text="title" :dataDate="dataDate" />
    <!-- v-bind ":" in "countries" prop to countries -->
    <CountrySelect @get-country="getCountryData" :countries="countries" />
    <br>
    <!-- v-bind in "stats" prop to stats -->
    <DataBoxes :stats="stats" />
    <!-- @click called to clear form once selected and remove selected country data -->
    <!-- "clearData" is a method called upon once "@click" is called by clicking the button -->
    <!-- v-if is called to show button only if country is selected -->
    <button 
      @click="clearData"
      v-if="stats.Country"
      class="bg-green-700 text-white rounded p-3 mt-10 focus:outline-none hover:bg-green-600">
        Clear
    </button>
    <br>
  </main>
  <!-- v-else is if it is loading then we'll show something else -->
  <main class="flex flex-col align-center justify-center text-center" v-else>
    <div class="text-gray-500 text-3xl mt-10 mb-6">
      Fetching Data
    </div>
    <!-- ":src" : is for binding the data with image -->
    <!-- "w-24" = width 24rem and "m-auto" = margin auto --> 
    <img :src="loadingImage" class="w-24 m-auto" alt="">
  </main>
</template>

<script>
import DataTitle from '@/components/DataTitle';
import DataBoxes from '@/components/DataBoxes';
import CountrySelect from '@/components/CountrySelect';

export default {
  name: 'Home',
  components: {
    DataTitle,
    DataBoxes,
    CountrySelect,
  },
  // data is a function
  data() {
    return {
      loading: true,
      title: 'Global',
      dataDate: '',
      stats: {},
      countries: [],
      loadingImage: require('../assets/hourglass.gif'),
    }
  },
  // methods is an object and fetchCovidData is a function
  methods: {
    async fetchCovidData() {
      const res = await fetch('https://api.covid19api.com/summary')
      const data = await res.json()
      return data
    },
    // "country" gets passed in since it was called in "this.$emit('get-country', country)" in CountrySelect file 
    getCountryData(country) {
      // changes the "stats" from "Global" to the selected country
      this.stats = country
      // changes "title" of page from "Global" to the selected country
      this.title = country.Country
    },
    // "async" because making another request in this method
    async clearData() {
      this.loading = true
      // data is a variable and await makes it a promise
      const data = await this.fetchCovidData()
      // returns title back to Global and removes selected country, Global is in a string
      this.title = 'Global'
      // returns data back to Global data and not selected country
      this.stats = data.Global
      this.loading = false
    }
  },
  async created() {
    // data is a variable
    // this.fetchCovidData() is a promise
    const data = await this.fetchCovidData()
  
    // data.Date = "data" coming from the API with the "Date" value
    this.dataDate = data.Date
    // data.Date = "data" coming from the API with the "Global" value
    this.stats = data.Global
    this.countries = data.Countries
    this.loading = false
  },
}
</script>
