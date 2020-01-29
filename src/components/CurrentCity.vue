<template>
  <div class="weather-wrapper">
    <b-container fluid class="container-current wht-bg border marginbottom">
      <b-row>
        <b-col cols="5" class="text-left lineheight">
          <b-col cols="12" class="city-name">{{ city.name }}</b-col>
          <b-col cols="12" class="weather-description">{{ description }}</b-col>
        </b-col>
        <b-col cols="7" class="text-right city-temperature lineheight">
          <img :src="iconSRC()" alt="current-weather-icon" />
          <span class="text-left">
            {{ temp }}
            <sup>o</sup>C
          </span>
        </b-col>
      </b-row>
      <b-row>
        <b-col cols="5" class="text-left lineheight">
          <b-col cols="12" class="city-date">{{ datetime.date }}</b-col>
          <b-col cols="12" class="city-time">{{ datetime.time }}</b-col>
        </b-col>
        <b-col cols="7" class="text-right lh">
          <ul class="city-details">
            <li>Wind: {{ city.wind.speed }} m/s</li>
            <li>Humidity: {{ city.main.humidity }} %</li>
            <li>Precipitation (3 h): {{ precipitation }} mm</li>
          </ul>
        </b-col>
      </b-row>
    </b-container>
    <b-container fluid class="container-forecasts marginbottom">
      <b-row class="row-forecast">
        <Error v-if="err" componentName="forecast" />
        <Forecast v-for="forecast in forecasts" v-bind:key="forecast.dt" :forecast="forecast" />
      </b-row>
    </b-container>
  </div>
</template>

<script>
import { BCol, BRow, BContainer } from "bootstrap-vue";

import Forecast from "./ForecastCity";
import Error from "./Error";

import axios from "axios";
import moment from "moment";

export default {
  name: "CurrentCity",
  components: {
    BCol,
    BRow,
    BContainer,
    Forecast,
    Error
  },
  props: {
    city: Object
  },
  data() {
    return {
      forecasts: {},
      err: false
    };
  },
  methods: {
    async getForecast() {
      this.error = false;
      try {
        let result = await axios(
          process.env.VUE_APP_BASE +
            "forecast?id=" +
            this.city.id +
            "&units=metric&cnt=5&appid=" +
            process.env.VUE_APP_APIKEY
        );
        this.forecasts = result.data.list;
      } catch (error) {
        this.err = true;
      }
    },
    iconSRC() {
      return process.env.VUE_APP_IMGURL + this.city.weather[0].icon + "@2x.png";
    }
  },
  computed: {
    temp() {
      return Math.round(this.city.main.temp);
    },
    description() {
      return this.city.weather[0].description;
    },
    datetime: function() {
      // format UNIX time to UTC
      let utcFormat = moment.unix(this.city.dt);
      // set datetime object with seperated date and time
      let datetime = {
        date: utcFormat.format("MMMM Do"),
        time: utcFormat.format("HH:mm")
      };
      return datetime;
    },
    precipitation() {
      // check if rain or snow exists in forecast object else return 0
      if ("rain" in this.city)
        if ("3h" in this.city.rain) return Math.round(this.city.rain["3h"]);
      if ("snow" in this.city)
        if ("3h" in this.city.snow) return Math.round(this.city.snow["3h"]);
      return 0;
    }
  },
  mounted() {
    this.getForecast();
  }
};
</script>

<style scoped>
/* Rows */
.row {
  width: 100%;
  margin: 0px;
  align-items: flex-end;
}

.row:first-child {
  margin-bottom: 1rem;
  align-items: center;
}

.row-forecast {
  justify-content: space-between;
}

/* Columns */
.city-name {
  font-size: 19pt;
  color: #262626;
}

.weather-description {
  font-size: 13pt;
  color: #70757a;
  text-transform: capitalize;
}

.city-temperature {
  font-size: 26pt;
  color: #262626;
  align-items: center;
  display: flex;
  justify-content: flex-end;
}

.city-date {
  font-size: 15pt;
  color: #262626;
}

.city-time {
  font-size: 13pt;
  color: #70757a;
}

/* List items */
.city-details {
  font-size: 13pt;
  color: #70757a;
  list-style: none;
  margin-bottom: 0px;
  padding: 0rem;
}

/* Containers */
.container-current {
  padding: 0.5rem 1.25rem 1rem 1.25rem;
}

.container-forecasts {
  padding-left: 0px;
  padding-right: 0px;
}

/* Custom classes */
.lineheight {
  line-height: 1.5rem;
}
</style>
