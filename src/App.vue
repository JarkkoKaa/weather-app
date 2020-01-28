<template>
  <div id="app">
    <div id="app-header" class="wht-bg border">SÄÄTUTKA</div>
    <div class="app-container">
      <SelectCity @selectedCity="selectionHandler" :options="options" />
      <div v-if="!isLoading" class="weather-wrapper">
        <CurrentCity v-for="city in cities" :key="city.id" :city="city" />
      </div>
      <div v-if="isLoading" class="spinner-grow" style="width: 3rem; height: 3rem;" role="status">
        <span class="sr-only">Loading...</span>
      </div>
    </div>
  </div>
</template>

<script>
import CurrentCity from "./components/CurrentCity.vue";
import SelectCity from "./components/SelectCity";

import axios from "axios";
export default {
  name: "app",
  components: {
    CurrentCity,
    SelectCity
  },
  data() {
    return {
      options: [
        { name: "All cities", value: "all_cities" },
        { name: "Tampere", value: 634964 },
        { name: "Jyväskylä", value: 655195 },
        { name: "Kuopio", value: 650225 },
        { name: "Helsinki", value: 658225 }
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
        this.getAll(listOfIDs);
      } else {
        // only one city is selected
        this.getSelected(value);
      }
    },
    // get All cities
    async getAll(IDs) {
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
    },
    // get single city
    async getSelected(id) {
      this.cities = [];
      this.isLoading = true;
      let result = await axios
        .get(
          process.env.VUE_APP_BASE +
            "weather?id=" +
            id +
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
      this.cities.push(result.data);
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
body {
  font-family: "Arial Regular";
  background-color: #f8f9fa;
}

#app {
  align-items: center;
  display: flex;
  flex-direction: column;
}

#app-header {
  font-size: 23pt;
  color: #262626;
  width: 100%;
  text-align: center;
  padding: 1rem;
}

.app-container {
  width: 89.33%;
  align-items: center;
  display: flex;
  flex-direction: column;
}

.border {
  border: 1px solid #e6e6e6;
  border-radius: 10px;
  margin-bottom: 2vh;
}

.row-forecast {
  justify-content: space-between;
}

select {
  margin-bottom: 2vh;
}

.wht-bg {
  background: #ffffff;
}

.weather-wrapper {
  width: 100%;
}
</style>
