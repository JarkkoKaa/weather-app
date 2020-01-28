<template>
  <b-col cols="2" class="wht-bg border text-center">
    <b-col cols="12">
      <b-row class="time">{{ time }}</b-row>
      <b-row>
        <img :src="iconSRC()" alt="forecast-icon" />
      </b-row>
      <b-row class="temperature">
        {{ temp }}
        <sup>o</sup>C
      </b-row>
    </b-col>
    <b-col cols="12" class="details">
      <b-row>{{ forecast.wind.speed }} m/s</b-row>
      <b-row>{{ forecast.main.humidity }} %</b-row>
      <b-row>{{ precipitation }} mm</b-row>
    </b-col>
  </b-col>
</template>

<script>
import { BCol, BRow } from "bootstrap-vue";
export default {
  name: "ForecastCity",
  components: {
    BCol,
    BRow
  },
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
    temp() {
      return Math.round(this.forecast.main.temp);
    },
    precipitation() {
      // check if rain or snow exists in forecast object else return 0
      if ("rain" in this.forecast)
        if ("3h" in this.forecast.rain)
          return Math.round(this.forecast.rain["3h"]);
      if ("snow" in this.forecast)
        if ("3h" in this.forecast.snow)
          return Math.round(this.forecast.snow["3h"]);
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
/* Columns */
.details {
  font-size: 10pt;
  color: #70757a;
  background-color: #e5f6fd;
}

.col-2 {
  flex: 0 0 19%;
  max-width: 19%;
}

.col-12 {
  padding: 0.5rem;
}

/* Rows */
.time {
  font-size: 13pt;
  color: #70757a;
}

.temperature {
  font-size: 15pt;
  color: #70757a;
}

.row {
  display: block;
}

.row-forecast {
  justify-content: space-between;
}
</style>
