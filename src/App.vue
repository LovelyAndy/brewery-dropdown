<template>
  <div id="app">
    <h1>Breweries List</h1>
    <form class="_form" @submit.prevent="submit">
      City
      <select name="cities" id="_cities" v-model="selectedCity">
        <option value="">All</option>
        <option value="milwaukee">Milwaukee</option>
      </select>
      <!-- <div>{{ selectedCity }}</div> -->
      Type
      <select name="types" id="_types" v-model="selectedType">
        <option value="">All</option>
        <option value="micro">Micro</option>
        <!-- <option value="micro">Micro</option>
        <option value="micro">Micro</option>
        <option value="micro">Micro</option>
        <option value="micro">Micro</option> -->
      </select>
      <!-- <div>{{ selectedType }}</div> -->
      <button type="submit" class="_btn" alt="submit form">Submit</button>
      <ul class="_selected">
        <li v-for="city in cities">
          <span>{{ city.city }}</span>
          <br />
          <span>{{ city.brewery_type }}</span>
        </li>
      </ul>
    </form>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      cities: [],
      selectedCity: '',
      selectedType: '',
    }
  },
  components: {},
  async created() {
    try {
      this.submit()
    } catch (e) {
      console.error(error)
    }
  },
  // computed: {
  //   selectedCities() {
  //     console.log(`selectedCity → `, this.selectedCity)
  //     return this.cities.filter((city) => {
  //       city.toLowerCase().startsWith(this.selectedCity.toLowerCase())
  //     })
  //   },
  //   selectedTypes() {
  //     console.log(`selectedType → `, this.selectedType)
  //     return (this.filteredTypes = this.types.filter((type) => {
  //       type.toLowerCase().startsWith(this.selectedType.toLowerCase())
  //     }))
  //   },
  // },
  //dynamically creating a new queury to the server. Theres no way to get all of the city names
  // add pagination****** (make it set a variable that gets set as a query param) -- page
  // The offset or page of breweries to return.
  methods: {
    async submit() {
      let per_page = 50
      let params = { per_page }
      if (this.selectedCity !== '') {
        params.by_city = this.selectedCity
      }
      if (this.selectedType !== '') {
        params.by_type = this.selectedType
      }
      const res = await axios.get('', { params })
      // const typeRes = await axios.get('/by_type')
      this.cities = res.data
      this.types = res.data
    },
  },
}
</script>

<style lang="sass">
#app
  font-family: Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing: antialiased
  -moz-osx-font-smoothing: grayscale
  text-align: center
  color: #2c3e50
  margin-top: 60px

li
  list-style-type: none
</style>
