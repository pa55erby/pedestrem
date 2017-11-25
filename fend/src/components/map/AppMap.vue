<template>
  <div>
    <div>
      <v-text-field
        box
        label="Search"
        hint="Search a location..."
        v-model="mapSearchText"
        v-on:keyup="geolocateSearchText"
      ></v-text-field>
      <v-list v-if="showSuggestions">
        <v-list-tile v-for="location in searchResults" :key="location.label" v-on:click="e => setMapLocation(location)">
          <v-list-tile-title>{{ location.label }}</v-list-tile-title>
        </v-list-tile>
      </v-list>
    </div>
    <div id="app-map"></div>
  </div>
</template>

<script>
/* global L */
import { OpenStreetMapProvider } from 'leaflet-geosearch'

const mapProvider = new OpenStreetMapProvider()
const mapAccessToken = 'pk.eyJ1IjoicGE1NWVyYnkiLCJhIjoiY2phZGozOHlpMW55ZjJ3bmlia25hYnBscCJ9.7eyVgUWa9aUFYjt0zDN9mg'
const mapAttribution = 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>'
const mapboxApiUrl = 'https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}'

export default {
  name: 'HelloWorld',
  data () {
    return {
      appMap: null,
      mapSearchText: '',
      searchResults: [],
      showSuggestions: false
    }
  },
  mounted () {
    this.loadAppMap()
  },
  methods: {
    loadAppMap () {
      this.appMap = L.map('app-map').setView([0, 0], 2)

      L.tileLayer(mapboxApiUrl, {
        attribution: mapAttribution,
        maxZoom: 18,
        id: 'mapbox.streets',
        accessToken: mapAccessToken
      }).addTo(this.appMap)
    },
    setMapLocation (location = null) {
      if (!this.searchResults.length && !location) return
      if (!location) {
        location = this.searchResults[0]
      }
      let zoom = 15
      if (location.raw.type === 'city') zoom = 10

      this.appMap.setView([location.y, location.x], zoom)
      this.showSuggestions = false
    },
    geolocateSearchText (e) {
      if (!this.mapSearchText.length) {
        this.showSuggestions = false
        return
      }

      if (e.code === 'Enter') {
        this.setMapLocation()
        return
      }

      mapProvider.search({ query: this.mapSearchText })
        .then(result => {
          this.searchResults = result
          this.showSuggestions = true
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass" scoped>
#app-map
  height: 650px
</style>
