<template>
  <div class="col-2 wht-bg border text-center">
    <div class="col-12">
      <div class="row time">{{ time }}</div>
      <div class="row">
        <img :src="iconSRC()" alt="forecast-icon" />
      </div>
      <div class="row temperature">
        {{ forecast.main.temp }}
        <sup>o</sup>C
      </div>
    </div>
    <div class="col-12 details">
      <div class="row">{{ forecast.wind.speed }} m/s</div>
      <div class="row">{{ forecast.main.humidity }} %</div>
      <div class="row">{{ precipitation }} mm</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ForecastCity",
  props: {
    forecast: Object
  },
  methods: {
    iconSRC() {
      return (
        process.env.VUE_APP_IMGURL + this.forecast.weather[0].icon + ".png"
      );
    }
  },
  computed: {
    precipitation() {
      // check if rain or snow exists in forecast object else return 0
      if ("rain" in this.forecast)
        if ("3h" in this.forecast.rain) return this.forecast.rain["3h"];
      if ("snow" in this.forecast)
        if ("3h" in this.forecast.snow) return this.forecast.snow["3h"];
      return 0;
    },
    time() {
      // handle datetime as a string, split time
      let time = this.forecast.dt_txt.split(" ");
      return time[1].slice(0, 5);
    }
  }
};
</script>

<style scoped>
.time {
  font-size: 13pt;
  color: #70757a;
}

.temperature {
  font-size: 15pt;
  color: #70757a;
}

.details {
  font-size: 10pt;
  color: #70757a;
  background-color: #e5f6fd;
}

.row {
  display: block;
}

.col-2 {
  padding-left: 0px;
  padding-right: 0px;
  flex: 0 0 19%;
  max-width: 19%;
}

.col-12 {
  padding: 0.5rem;
}
</style>