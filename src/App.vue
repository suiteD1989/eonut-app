<template>
  <div id="app">
    <div class="container is-fluid has-text-centered">
      <h1>Daragh Cassidy | Earth Observatory Natural Events Tracker</h1>
      <div>
        <DataTable :nasaData="this.nasaData"/>
      </div>
    </div>
  </div>
</template>

<script>

import 'bulma/css/bulma.css'

const axios = require('axios');
const openEvents = 'https://eonet.sci.gsfc.nasa.gov/api/v2.1/events?limit=25&status=open';
const closedEvents = 'https://eonet.sci.gsfc.nasa.gov/api/v2.1/events?limit=25&status=closed';

/**
* Fetch open/closed events from NASA 
*/
const getOpenEvents = axios.get(openEvents);
const getClosedEvents = axios.get(closedEvents);

import DataTable from './components/DataTable.vue';

export default {
  name: 'App',
  components: {
    DataTable
  },
  data () {
    return {
      nasaData: []
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
        console.log(this.nasaData);
        // use/access the results 
      })).catch(err => {
        console.log(err)
        // react on errors.
      })
    }
  },
  mounted () {
    this.fetchData();
  }
}
</script>

<style lang="scss">

</style>
