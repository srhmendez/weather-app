<template>
  <header>
    <h4>Drag The Marker To View Weather</h4>
  </header>

  <main>
    <GoogleMap
      api-key="AIzaSyDQrzLjoUJkj4oaxCxANQQX0Adcuk5gcUY"
      style="width: 50%; height: 530px"
      :center="center"
      :zoom="8"
    >
      <Marker :options="markerOptions" @dragend="mark" />
    </GoogleMap>
    <WeatherData v-bind:latitude="lat" v-bind:longitude="lon" />
  </main>
</template>

<script>
import { defineComponent } from 'vue';
import { GoogleMap, Marker } from 'vue3-google-map';
import WeatherData from './components/WeatherData.vue';

export default defineComponent({
  components: {
    GoogleMap,
    Marker,
    WeatherData,
  },
  setup() {
    const center = { lat: 40.7608, lng: -111.891 };
    const markerOptions = { position: center, draggable: true };

    return { center, markerOptions };
  },
  methods: {
    mark(event) {
      this.lat = event.latLng.lat();
      this.lon = event.latLng.lng();
    },
  },
  data() {
    return {
      lat: 40.7608,
      lon: -111.891,
    };
  },
});
</script>

<style>
@import './assets/base.css';

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 2rem;

  font-weight: normal;
}

header {
  line-height: 1.5;
}

h1 {
  color: rgb(36, 116, 172);
  text-align: center;
}
main {
  display: flex;
  margin-top: 1rem;
}
</style>
