<template>
  <div id="app">
    <div class="container">
      <h1>SpaceX Launches</h1>
      <DataControls v-on:refresh="refresh" v-on:changeFilter="updateFilter" :landed="landed" :reused="reused" :withReddit="withReddit" />
      <LaunchesList :launches="filteredLaunches" :isFetching="isFetching" :errorMessage="errorMessage" />
    </div>
  </div>
</template>

<script>
import DataControls from "./components/DataControls.vue"
import LaunchesList from "./components/LaunchesList.vue"
import { toPairs, some } from "lodash"

export default {
  name: "app",
  components: {
    DataControls,
    LaunchesList
  },
  data: function () {
    return {
      launches: [],
      landed: false,
      reused: false,
      withReddit: false,
      isFetching: false,
      errorMessage: ""
    }
  },
  computed: {
    filteredLaunches () {
      return this.launches.filter(launch => {
        let include = true
        if (this.landed && !this.didLand(launch)) include = false
        if (this.reused && !this.wasReused(launch)) include = false
        if (this.withReddit && !this.hasReddit(launch)) include = false
        return include
      })
    }
  },
  mounted () {
    this.fetchData()
  },
  methods: {
    refresh () {
      this.launches = []
      this.fetchData()
    },
    async fetchData () {
      this.isFetching = true
      try {
        const response = await fetch("https://api.spacexdata.com/v2/launches")
        const result = await response.json()
        this.isFetching = false
        this.launches = result
      } catch(error) {
        this.isFetching = false
        this.errorMessage = `An error occurred in fetching the launches: ${error.statusText}`
      }
    },
    updateFilter (eventObj) {
      this[eventObj.filter] = eventObj.value
    },
    hasReddit (launch) {
      return some(toPairs(launch.links), pair => {
        return pair[0].includes("reddit") && pair[1] !== null
      })
    },
    didLand (launch) {
      return some(launch.rocket.first_stage.cores, core => {
        return !!core.land_success
      })
    },
    wasReused (launch) {
      return !!launch.rocket.first_stage.cores[0].reused ||
      some(launch.rocket.second_stage.payloads, payload => {
        return payload.reused
      }) ||
      (launch.rocket.fairings && launch.rocket.fairings.reused) ||
      some(toPairs(launch.reuse), pair => {
        return pair[1]
      })
    }
  }
}
</script>

<style lang="scss">
@font-face {
  font-family: "geomanist";
  src: url("./assets/fonts/geomanist-regular-webfont.eot"); /* IE9 Compat Modes */
  src: url("./assets/fonts/geomanist-regular-webfont.eot?#iefix") format("embedded-opentype"), /* IE6-IE8 */
       url("./assets/fonts/geomanist-regular-webfont.woff2") format("woff2"), /* Super Modern Browsers */
       url("./assets/fonts/geomanist-regular-webfont.woff") format("woff"), /* Pretty Modern Browsers */
       url("./assets/fonts/geomanist-regular-webfont.ttf")  format("truetype"), /* Safari, Android, iOS */
       url("./assets/fonts/geomanist-regular-webfont.svg#svgFontName") format("svg"); /* Legacy iOS */
}

body {
  background: url("./assets/images/background.jpg") top center no-repeat;
  background-attachment: fixed;
  background-size: cover;
  padding: 0;
  margin: 0;
  font-family: "geomanist", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  text-align: center;
}

h1 {
  color: white;
  font-weight: normal;
  margin: 30px 0 38px 0;
  font-size: 64px;
  line-height: 1;
}

.container {
  margin: 0 auto 60px;
  max-width: 960px;
  width: 90%;
}
</style>
