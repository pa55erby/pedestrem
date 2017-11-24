<template>
  <div>
    <h1>Pedestrem</h1>
    <app-map></app-map>
  </div>
</template>

<script>
import AppMap from '@/components/map/AppMap'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  components: {
    AppMap
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass" scoped>
h1, h2
  font-weight: normal
ul
  list-style-type: none
  padding: 0
li
  display: inline-block
  margin: 0 10px
a
  color: #42b983
</style>
