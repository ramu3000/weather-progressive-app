<template>
  <div id="app">
   <mobile-layout
    :weather-data="weather">
   </mobile-layout>
  </div>
</template>

<script>

import MobileLayout from './components/MobileLayout'
import Metolib from '@fmidev/metolib';
import moment from 'moment'
import { API_KEY} from '../env.js'
window.moment = moment
//import axios from 'axios';
export default {
  name: 'App',
  components: {
    MobileLayout
  },
  data(){
    return {
      STORED_QUERY_OBSERVATION: "fmi::observations::weather::multipointcoverage",
      TEST_SERVER_URL: `http://data.fmi.fi/fmi-apikey/${API_KEY}/wfs`,
      weather: {},
    }
  },
  methods: {
    handleCallback(data, errors, caseName) {
        if (data) {
          console.log(data);
          this.weather = data;
        }
        if (errors.length > 0 || Object.keys(errors).length > 0) {
          console.log(errors, 'errors');
        }
    },
    testHkiTd() {
      console.log(moment.now())
      const dateNow = moment.now();
      const dateNowPlusTwoHours = moment(dateNow).add(2, 'hours');
      console.log(dateNowPlusTwoHours);
        //const self = this;
        // See API documentation and comments from connection source code of
        // Metolib.WfsConnection.getData function for the description
        // of function options parameter object and for the callback parameters objects structures.
        var connection = new Metolib.WfsConnection();
        if (connection.connect(this.TEST_SERVER_URL, this.STORED_QUERY_OBSERVATION)) {
          // Connection was properly initialized. So, get the data.
          connection.getData({
              requestParameter : "temperature",
              // Integer values are used to init dates for older browsers.
              // (new Date("2013-05-10T08:00:00Z")).getTime()
              // (new Date("2013-05-12T10:00:00Z")).getTime()
              begin : new Date(dateNow),
              end : new Date(dateNowPlusTwoHours),
              timestep : 60 * 60 * 1000,
              sites : "Kumpula",
              callback : (weatherData, errors) => {
                  // Handle the data and errors object in a way you choose.
                  // Here, we delegate the content for a separate handler function.
                  // See parser documentation from source code comments for more details.
                  this.handleCallback(weatherData, errors, "Helsinki td");
                  // Disconnect because the flow has finished.
                  connection.disconnect();
              }
          });
      }
    }
  },
  mounted(){
    this.testHkiTd()
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
