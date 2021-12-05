<template>
  <!-- v-if is a directive to conditionally render a block -->
  <!-- "!loading" = if it's not loading we'll show our data -->
  <main v-if="!loading">
    Show Data
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

export default {
  name: 'Home',
  components: {},
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
      const res = await fetch('https://api.coivd19api.com/summary')
      const data = await res.json()
      return data
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
