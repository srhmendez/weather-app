<template>
  <div id="weather-container">
    <h1 class="location" v-if="this.weatherIsLoaded">
      {{ this.weatherData.name }}
    </h1>
    <div id="temperature-container">
      <h2 id="temperature-number" v-if="this.weatherIsLoaded">
        {{ this.weatherData.main.temp }}˚F
      </h2>
    </div>
    <div id="weather-info-container">
      <h3 id="condition" class="weather-info" v-if="this.weatherIsLoaded">
        <span class="weatherNumbers">Condition: </span
        >{{ this.weatherData.weather[0].main }}
      </h3>
      <h3 id="humidity" class="weather-info" v-if="this.weatherIsLoaded">
        <span class="weatherNumbers">Humidity: </span
        >{{ this.weatherData.main.humidity }}%
      </h3>
      <h3 id="sunrise" class="weather-info" v-if="this.weatherIsLoaded">
        <span class="weatherNumbers">Sunrise: </span
        >{{ convertTime(this.weatherData.sys.sunrise) }}
      </h3>
      <h3 id="sunset" class="weather-info" v-if="this.weatherIsLoaded">
        <span class="weatherNumbers">Sunset: </span
        >{{ convertTime(this.weatherData.sys.sunset) }}
      </h3>
      <h3 id="wind-speed" class="weather-info" v-if="this.weatherIsLoaded">
        <span class="weatherNumbers">Wind Speed:</span>
        {{ this.weatherData.wind.speed }} MPH
      </h3>
      <h3 id="wind-direction" class="weather-info" v-if="this.weatherIsLoaded">
        <span class="weatherNumbers">Wind Direction:</span>
        {{ this.weatherData.wind.deg }} Degrees
      </h3>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      weatherData: {},
      weatherIsLoaded: false,
    };
  },
  props: {
    latitude: Number,
    longitude: Number,
  },
  methods: {
    loadWeather() {
      fetch(
        `https://api.openweathermap.org/data/2.5/weather?lat=${this.latitude}&lon=${this.longitude}&appid=1a1f8422d1097090cfc9a2b1480ad453&units=imperial`
      )
        .then(async response => {
          if (response.ok) {
            return response.json();
          } else {
            return Promise.reject();
          }
        })
        .then(data => {
          this.weatherData = data;
        })
        .then(() => {
          this.weatherIsLoaded = true;
        });
    },
    convertTime(timeOfDay) {
      let dt = new Date(timeOfDay * 1000);
      let hr = dt.getHours();
      let m = '0' + dt.getMinutes();
      let s = '0' + dt.getSeconds();
      let timeStamp;
      if (hr < 12) {
        timeStamp = 'AM';
      } else {
        timeStamp = 'PM';
      }
      return hr + ':' + m.substr(-2) + ':' + s.substr(-2) + ' ' + timeStamp;
    },
  },
    watch: {
    latitude() {this.loadWeather()},
    longitude() {this.loadWeather()},
  },
  beforeMount() {
    this.loadWeather();
  },
};
</script>

<style>
h1.location {
  padding-top: 4%;
  font-size: 3rem;
  color: blue;
}
h3 {
  display: flex;
}
h3 span {
  width: 50%;
  display: flex;
  justify-content: end;
  padding-right: 10%;
}
#weather-container {
  background-color: rgb(173, 213, 255);
  width: 50%;
  display: flex;
  justify-content: flex-start;
  flex-direction: column;
}
#temperature-container {
  margin: 0 auto;
  background-color: rgb(226, 243, 255);
  width: 95%;
  height: 5rem;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 8px;
  margin-top: 3%;
}
#temperature-number {
  font-size: 4rem;
  padding: 1%;
}
#weather-info-container {
  background: aliceblue;
  margin: 2%;
  padding: 4%;
  border-radius: 8px;
}
.weather-info {
  padding: 2% 0;
}
</style>
