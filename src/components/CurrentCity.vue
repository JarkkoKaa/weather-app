<template>
  <div class="weather-wrapper">
    <div class="container-fluid container-current wht-bg border">
      <div class="row">
        <div class="col-4 text-left">
          <div class="col-12 city-name">{{ city.name }}</div>
          <div class="col-12 weather-description">{{ description }}</div>
        </div>
        <div class="col-8 text-right city-temperature">
          <img :src="iconSRC()" alt="current-weather-icon" />
          <span class="text-left">
            {{ temp }}
            <sup>o</sup>C
          </span>
        </div>
      </div>
      <div class="row">
        <div class="col-3 text-left">
          <div class="col-12"></div>
          <div class="col-12 city-date">{{ datetime.date }}</div>
          <div class="col-12 city-time">{{ datetime.time }}</div>
        </div>
        <div class="col-9 text-right">
          <ul class="city-details">
            <li>Wind: {{ city.wind.speed }} m/s</li>
            <li>Humidity: {{ city.main.humidity }} %</li>
            <li>Precipitation (3 h): {{ precipitation }} mm</li>
          </ul>
        </div>
      </div>
    </div>
    <div class="container-fluid container-forecasts">
      <div class="row row-forecast">
        <Forecast v-for="forecast in forecasts" v-bind:key="forecast.dt" :forecast="forecast" />
      </div>
    </div>
  </div>
</template>

<script>
import Forecast from "./ForecastCity";
import axios from "axios";
import moment from "moment";

export default {
  name: "CurrentCity",
  components: {
    Forecast
  },
  props: {
    city: Object
  },
  data() {
    return {
      forecasts: {}
    };
  },
  methods: {
    async getForecast() {
      let result = await axios(
        process.env.VUE_APP_BASE +
          "forecast?id=" +
          this.city.id +
          "&units=metric&cnt=5&appid=" +
          process.env.VUE_APP_APIKEY
      )
        .then(response => {
          return response.data.list;
        })
        .catch(error => {
          console.log(error);
        });
      this.forecasts = result;
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
      let utcFormat = moment().utc(this.city.dt);
      // set datetime object with seperated date and time
      let datetime = {
        date: moment(utcFormat).format("MMMM Do"),
        time: moment(utcFormat).format("HH:mm")
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
.col-3,
.col-4,
.col-8,
.col-9,
.col-12 {
  line-height: 1.5rem;
}

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
}

/* Containers */
.container-current {
  padding: 1.25rem 1rem 1rem 1rem;
}

.container-forecasts {
  padding-left: 0px;
  padding-right: 0px;
}
</style>
