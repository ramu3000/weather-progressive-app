<template>
    <div class="wrapper">
         <v-toolbar light color="primary">
            <v-toolbar-side-icon></v-toolbar-side-icon>
            <v-toolbar-title class="black--text">Title</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-btn icon>
                <v-icon>my_location</v-icon>
            </v-btn>
            <v-btn icon>
                <v-icon>refresh</v-icon>
            </v-btn>
            <v-btn icon>
                 <v-icon>more_vert</v-icon>
            </v-btn>
        </v-toolbar>
        <div class="weather-info-wrapper" 
            v-if="weatherData.locations"
            >
            <div class="location">
                 {{ weatherData.locations["0"].info.name}}
            </div>
            <div class="temperature"> 
                <v-icon x-large>fa-thermometer-three-quarters</v-icon>
                    {{weatherData.locations["0"].data.temperature.timeValuePairs["0"].value}}
            </div>
            <div class="time">
                {{ weatherData.locations["0"].data.temperature.timeValuePairs["0"].time | moment}}
            </div>
 
        </div> 
        <button @click="unixToDate"></button>
    </div>
</template>

<script>
import moment from "moment";
export default {
  props: ["weatherData"],
  data() {
    return {};
  },
  methods: {
    unixToDate() {
      this.weatherData.locations["0"].data.temperature.timeValuePairs.forEach(values => {
        console.log(
          moment(values.time).format("HH:mm DD.MM.YYYY"),
          values.value
        );
      });
    }
  },
  filters: {
    moment(date) {
      return moment(date).format("HH:mm DD.MM.YYYY");
    }
  }
};
</script>

<style scoped>
.wrapper {
  max-width: 600px;
  height: 800px;
  margin: 0 auto;
  background-color: aqua;
}
.weather-info-wrapper {
    margin-top:20px;
}
.weather-info-wrapper .temperature {
    font-size:32px;
}
</style>
