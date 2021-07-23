<template>
  <div id="app">
    <h1>Breweries List</h1>
    <form class="_form">
      City
      <select name="cities" id="_cities" v-model="selectedCity">
        <option value="all">All</option>
        <option value="cityNames" v-for="city in cities" :key="city.id">{{ city.city }}</option>
      </select>
      <div>{{ selectedCity }}</div>
      Type
      <select name="types" id="_types" v-model="selectedType">
        <option value="all">All</option>
        <option value="types" v-for="type in types" :key="type.id">{{ type.brewery_type }}</option>
      </select>
      <div>{{ selectedType }}</div>
      <button value="save" class="_btn" alt="submit form" @click="submit">Submit</button>
      <!-- <ul class="_selected">
        <li v-for="city in selectedCities">
          <span>{{ city.city }}</span>
        </li>
        <li v-for="type in selectedTypes">
          <span>{{ type.type }}</span>
        </li>
      </ul> -->
      <ul>
        <li v-for="cities in filteredCities">{{ city }}</li>
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
      types: [],
      selectedCity: '',
      selectedType: '',
    }
  },
  components: {},
  async created() {
    try {
      const cityRes = await axios.get('https://api.openbrewerydb.org/breweries?/by_city')
      const typeRes = await axios.get('https://api.openbrewerydb.org/breweries?/by_type')
      this.cities = cityRes.data
      this.types = typeRes.data
    } catch (e) {
      console.error(error)
    }
  },
  computed: {
    selectedCities() {
      console.log(`selectedCity → `, this.selectedCity)
      return this.cities.filter((city) => {
        city.toLowerCase().startsWith(this.selectedCity.toLowerCase())
      })
    },
    selectedTypes() {
      console.log(`selectedType → `, this.selectedType)
      return (this.filteredTypes = this.types.filter((city) => {
        city.toLowerCase().startsWith(this.selectedType.toLowerCase())
      }))
    },
  },
  methods: {
    submit() {
      console.log(1)
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
