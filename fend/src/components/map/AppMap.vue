<template>
  <div id="app-map"></div>
</template>

<script>
/* global L */
const mapAccessToken = 'pk.eyJ1IjoicGE1NWVyYnkiLCJhIjoiY2phZGozOHlpMW55ZjJ3bmlia25hYnBscCJ9.7eyVgUWa9aUFYjt0zDN9mg'
const mapAttribution = 'Map data &copy; <a href="http://openstreetmap.org">OpenStreetMap</a> contributors, <a href="http://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="http://mapbox.com">Mapbox</a>'

export default {
  name: 'HelloWorld',
  data () {
    return {
      appMap: null
    }
  },
  mounted () {
    this.loadAppMap()
  },
  methods: {
    loadAppMap () {
      this.appMap = L.map('app-map').setView([40.7128, -74.0060], 13)

      L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}', {
        attribution: mapAttribution,
        maxZoom: 18,
        id: 'mapbox.streets',
        accessToken: mapAccessToken
      }).addTo(this.appMap)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass" scoped>
#app-map
  height: 650px
</style>
