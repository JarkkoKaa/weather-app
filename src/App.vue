<template>
  <div id="app">
    <div id="app-header" class="wht-bg marginbottom">Säätutka</div>
    <b-container fluid class="app-container">
      <SelectCity @selectedCity="selectionHandler" :options="options" />
      <div v-if="!isLoading" class="weather-wrapper">
        <CurrentCity v-for="city in cities" :key="city.id" :city="city" />
      </div>
      <div v-if="isLoading" class="spinner-grow" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </b-container>
  </div>
</template>

<script>
import { BContainer } from "bootstrap-vue";

import CurrentCity from "./components/CurrentCity.vue";
import SelectCity from "./components/SelectCity";

import axios from "axios";
export default {
  name: "app",
  components: {
    BContainer,
    CurrentCity,
    SelectCity
  },
  data() {
    return {
      options: [
        { name: "All cities", value: "all_cities" },
        { name: "Helsinki", value: 658225 },
        { name: "Jyväskylä", value: 655195 },
        { name: "Kuopio", value: 650225 },
        { name: "Tampere", value: 634964 }
      ],
      cities: [],
      isLoading: true
    };
  },
  methods: {
    selectionHandler(value) {
      if (value == "all_cities") {
        // as a page loads: all cities will get weather and forecast
        let listOfIDs = "";
        // list is short so enumerate it o(n)
        for (let i = 1; i < this.options.length; i++) {
          if (i > 1) listOfIDs += ",";
          listOfIDs += this.options[i].value;
        }
        this.getSelected(listOfIDs);
      } else {
        // only one city is selected
        this.getSelected(value);
      }
    },
    // get all cities and selected cities -fixes non scandic characters
    async getSelected(IDs) {
      this.cities = [];
      this.isLoading = true;
      let result = await axios
        .get(
          process.env.VUE_APP_BASE +
            "group?id=" +
            IDs +
            "&units=metric" +
            "&appid=" +
            process.env.VUE_APP_APIKEY
        )
        .then(response => {
          return response;
        })
        .catch(error => {
          console.log(error);
        });
      this.cities = result.data.list;
      //  this.selectedWeather = result.data;
      this.isLoading = false;
    }
  },
  mounted() {
    // when page loads run this as a default
    this.selectionHandler("all_cities");
  }
};
</script>

<style>
/* HTML elements */
body {
  font-family: "Arial Regular" !important;
  background-color: #f8f9fa !important;
}

select {
  margin-bottom: 1rem;
}

/* App */
#app {
  align-items: center;
  display: flex;
  flex-direction: column;
}

/* Header */
#app-header {
  font-size: 23pt;
  color: #262626;
  width: 100%;
  text-align: center;
  padding: 1.25rem;
  border: 1px solid #e6e6e6;
}

/* Global containers */
.app-container {
  align-items: center;
  display: flex;
  flex-direction: column;
}

.container-fluid {
  width: 99% !important;
}

/* Border and background */
.border {
  border: 1px solid #e6e6e6 !important;
  border-radius: 10px;
}

.wht-bg {
  background: #ffffff;
}

/* Global wrapper */
.weather-wrapper {
  width: 100%;
}

/* Custom bootstrap */
.spinner-grow {
  width: 6rem;
  height: 6rem;
}

.col-2,
.col-3,
.col-4,
.col-5,
.col-7,
.col-8,
.col-9,
.col-12 {
  padding: 0px !important;
}

/* Custom classes */
.marginbottom {
  margin-bottom: 1.15rem;
}
</style>
