<template>
  <div id="app">
    <div id="app-header" class="wht-bg border">SÄÄTUTKA</div>
    <div class="app-container">
      <SelectCity @selectedCity="getSelected" :options="options" />
      <CurrentCity v-if="!isLoading" :city="selectedWeather" />
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
        { name: "Tampere", value: 634964 },
        { name: "Jyväskylä", value: 655195 },
        { name: "Kuopio", value: 650225 },
        { name: "Helsinki", value: 658225 }
      ],
      selectedWeather: {},
      isLoading: true
    };
  },
  methods: {
    async getSelected(val) {
      this.isLoading = true;
      let result = await axios
        .get(
          process.env.VUE_APP_BASE +
            "weather?id=" +
            val +
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
      this.selectedWeather = result.data;
      this.isLoading = false;
      console.log(this.selectedWeather);
    }
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
</style>
