<template>
  <div class="row">
    <h1>Vehicle data</h1>
    <div class="flex-container">
      <div class="map-container">
        <MapContainer :coordinates="parsedCoordinates"/>
      </div>
      <div class="flex-item">
        <ProgressBar :label="'Current speed'" :value="vehicleData.speed" :unit="'km/h'" />
        <ProgressBar :label="'State of charge'" :value="vehicleData.soc" :unit="'%'" />
        <div class="indicator-list">
          <div class="indicator-list__item">
            <h3>Energy</h3> <span>{{ vehicleData.energy }}</span>
          </div>
          <div class="indicator-list__item">
            <h3>Odometer</h3> <span>{{ vehicleData.odo }}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import MapContainer from './components/MapContainer.vue';
import ProgressBar from './components/Ui/ProgressBar.vue';

export default {
  name: 'App',
  components: {
    MapContainer,
    ProgressBar
  },

  data() {
    return {
      connection: null,
      vehicleData: {
        speed: 0,
        soc: 0,
      },
    }
  },

  computed: {
    parsedCoordinates() {
      if (this.vehicleData.gps) {
        const [lat, lng] = this.vehicleData.gps.split('|').map(parseFloat);
        return {lat, lng};
      }
      return { lat: 52.09126663208008, lng: 5.122183322906494};
    }
  },

  created() {
    this.connection = new WebSocket('ws://localhost:3000/')

    this.connection.onmessage = (event) => {
      this.vehicleData = JSON.parse(event.data);
    }

    this.connection.onopen = (event) => {
        console.log(event)
    }
  },
}
</script>

<style>
#app {
  font-family: var(--base-font-family);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: var(--color-darkgrey);
  min-height: 100vh;
  color: var(--color-white);
  height: 100%;
}
</style>
