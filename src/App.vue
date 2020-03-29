<template>
  <div id="app">
    <Heading />
    <div class="container">
      <div class="row">
        <div class="col loading">
          <LoadingCircle v-if="!this.dataLoaded"/>
        </div>
      </div>
    </div>
    <DataTable :nasaData="this.nasaData"/>
  </div>
</template>

<script>

import 'bootstrap-4-grid/css/grid.min.css'

const axios = require('axios');
const openEvents = 'https://eonet.sci.gsfc.nasa.gov/api/v2.1/events?limit=25&status=open';
const closedEvents = 'https://eonet.sci.gsfc.nasa.gov/api/v2.1/events?limit=25&status=closed';

/**
* Fetch open/closed events from NASA 
*/
const getOpenEvents = axios.get(openEvents);
const getClosedEvents = axios.get(closedEvents);

import DataTable from './components/DataTable.vue';
import Heading from './components/Heading.vue';
import LoadingCircle from 'vue-loading-spinner/src/components/Circle';

export default {
  name: 'App',
  components: {
    Heading,
    DataTable,
    LoadingCircle
  },
  data () {
    return {
      nasaData: [],
      dataLoaded: false
    }
  },
  methods: {
    /**
    * This methods queries the NASA eonet enpoint and returns the data 
    */
    fetchData () {
      axios.all([getOpenEvents, getClosedEvents]).then(axios.spread((...res) => {
        const openEventsRes = res[0].data.events;
        const closedEventRes = res[1].data.events;
        this.nasaData = openEventsRes.concat(closedEventRes);
        this.dataLoaded = true;
      })).catch(err => {
        console.log(err)
      })
    }
  },
  mounted () {
    this.fetchData();
  }
}
</script>

<style lang="scss">
  #app {
    background: #f7f7f7;
    height: 100vh;
  }
  body {
    margin: 0;
    font-family: 'Cabin', sans-serif;
    overflow-x: hidden;
    overflow-y: scroll;
    color: rgba(8, 29, 31, 0.8);
  }
  // Global
  .blur {
    transition: all .25s ease-in-out;
    filter: blur(5px);
  }
  .loading {
    padding-top: 3em;
    text-align: center;
  }
  // Overrides
  a:link {
    text-decoration: none;
  }
  .row {
    margin-left: 0;
    margin-right: 0;
  }
</style>
