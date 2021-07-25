<template>
  <div id="app">
    <h1>Milwaukee Breweries List</h1>
    <form class="_form" @submit.prevent="submit">
      City
      <select name="breweries" id="_breweries" v-model="selectedCity">
        <option value="">All</option>
        <option value="milwaukee">Milwaukee</option>
      </select>
      <!-- <div>{{ selectedCity }}</div> -->
      Type
      <select name="types" id="_types" v-model="selectedType">
        <option value="">All</option>
        <option value="micro">Micro</option>
        <option value="nano">Nano</option>
        <option value="regional">Regional</option>
        <option value="brewpub">Brewpub</option>
        <option value="large">Large</option>
        <option value="planning">Planning</option>
        <option value="bar">Bar</option>
        <option value="contract">Contract</option>
        <option value="proprietor">Proprietor</option>
        <!-- <option value="closed">Closed</option> -->
      </select>
      <button type="submit" class="_btn" alt="submit form">Submit</button>
      <ul class="_selected">
        <li
          class="_brewery"
          v-for="brewery in visibleBreweries"
          :key="brewery.id"
          :visibleBreweries="visibleBreweries"
          :currentPage="currentPage"
        >
          <div v-if="brewery.state">
            <div class="_title">State:</div>
            <div class="_info">{{ brewery.state }}</div>
          </div>
          <div v-if="brewery.name">
            <div class="_title">Brewery:</div>
            <div class="_info">{{ brewery.name }}</div>
          </div>
          <div v-if="brewery.brewery_type">
            <div class="_title">Type of Brewery:</div>
            <div class="_info">{{ brewery.brewery_type }}</div>
          </div>
          <div v-if="brewery.street">
            <div class="_title">Address:</div>
            <div class="_info">{{ brewery.street }}</div>
          </div>
          <div v-if="brewery.phone">
            <div class="_title">Phone Number:</div>
            <div class="_info">{{ brewery.phone }}</div>
          </div>
          <div v-if="brewery.website_url">
            <div class="_title">Website:</div>
            <a :href="brewery.website_url" class="_info" target="_blank" rel="noopener">{{
              brewery.website_url
            }}</a>
          </div>
        </li>
      </ul>
    </form>
    <Pagination
      v-if="visibleBreweries.length"
      :visibleBreweries="visibleBreweries"
      :currentPage="currentPage"
      :perPage="perPage"
      @page-update="updatePage"
    />
  </div>
</template>

<script>
import Pagination from './components/Pagination.vue'
import { BASE_URL } from './config.js'

export default {
  name: 'App',
  data() {
    return {
      selectedCity: '',
      selectedType: '',
      currentPage: 1,
      perPage: 10,
      visibleBreweries: [],
    }
  },
  components: { Pagination },
  //dynamically creating a new queury to the server, becasuse there's no way to get ALL of the city names since the API only allows up to 50 in one call. So I limited it to a few breweries.
  // add pagination****** (make it set a variable that gets set as a query param) -- page
  // The offset or page of breweries to return.
  methods: {
    async submit() {
      let per_page = this.perPage
      let page = this.currentPage
      let params = { per_page, page }
      if (this.selectedCity !== '') {
        params.by_city = this.selectedCity
      }
      if (this.selectedType !== '') {
        params.by_type = this.selectedType
      }
      const res = await axios.get(BASE_URL, { params })
      this.visibleBreweries = res.data
      console.log(`this.visibleBreweries → `, this.visibleBreweries)
      console.log(`this.currentPage → `, this.currentPage)
      this.types = res.data
      console.log(`clicked → `, 'clicked')
      // if result > 10 and are no more breweries
    },
    updatePage(pageNum) {
      this.currentPage = pageNum
      this.submit()
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

._brewery
  display: flex
  flex-direction: column
  list-style-type: none
  border: solid 2px black
  border-radius: 1em
  padding: 1em
  margin-bottom: 2em
._title
  font-weight: bold
._info
</style>
