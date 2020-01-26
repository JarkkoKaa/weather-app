<template>
  <div class="container border">
    <div class="row">
      <div class="col text-left">
        <div class="col-12">
          <span class="city-name">Tampere</span>
        </div>
        <div class="col-12">
          <span class="weather-description">Scattered clouds</span>
        </div>
      </div>
      <div class="col text-right city-temperature">
        0
        <sup>o</sup>C
      </div>
    </div>
    <div class="row">
      <div class="col-4 text-left">
        <div class="col-12">
          <span class="city-date">May 2nd</span>
        </div>
        <div class="col-12">
          <span class="city-time">00:00</span>
        </div>
      </div>
      <div class="col-8 text-right city-details">
        <ul class="city-details">
          <li>Wind: 0 m/s</li>
          <li>Humidity: 0 %</li>
          <li>Precipitation (3 h): 0 mm</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CurrentCity"
};
</script>

<style scoped>
.row {
  width: 100%;
  margin: 0px;
}

.row:first-child {
  margin-bottom: 10vh;
}

.col,
.col-4,
.col-8,
.col-12 {
  padding: 0px;
}

.city-name {
  font-size: 19pt;
  color: #262626;
}

.weather-description {
  font-size: 13pt;
  color: #70757a;
}

.city-temperature {
  font-size: 26pt;
  color: #262626;
}

.city-date {
  font-size: 15pt;
  color: #262626;
}
.city-time {
  font-size: 13pt;
  color: #70757a;
}

.city-details {
  font-size: 13pt;
  color: #70757a;
  list-style: none;
  margin-bottom: 0px;
}

.container {
  background: #ffffff;
}
</style>
