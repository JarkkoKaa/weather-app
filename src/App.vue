<template>
  <div id="app">
    <div id="app-header" class="border">SÄÄTUTKA</div>
    <div class="app-container">
      <SelectCity />
      <CurrentCity />
      <div class="container">
        <div class="row row-forecast">
          <Forecast />
          <Forecast />
          <Forecast />
          <Forecast />
          <Forecast />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CurrentCity from "./components/CurrentCity.vue";
import SelectCity from "./components/SelectCity";
import Forecast from "./components/ForecastCity";
export default {
  name: "app",
  components: {
    CurrentCity,
    SelectCity,
    Forecast
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
  background-color: #ffffff;
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
}

.border {
  border-radius: 10px;
  margin-bottom: 2vh;
}

.row-forecast {
  justify-content: space-between;
}

select {
  margin-bottom: 2vh;
}
</style>
