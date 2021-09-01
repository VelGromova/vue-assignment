<template>
  <div>
    <h1>Vehicle data</h1>
    <div class="map-container">
      <MapContainer :coordinates="parsedCoordinates"/>
    </div>
    <div class="progress-bar">
      <h3>Current speed: </h3>
      <span>{{ vehicleData.speed }} km/h</span>
    </div>
    <div class="progress-bar">
      <h3>State of charge: </h3>
      <span>{{ vehicleData.soc }} %</span>
    </div>
    <div class="indicator-list">
      <div class="indicator-list__item">
        <span>Energy</span> {{ vehicleData.energy }}
      </div>
      <div class="indicator-list__item">
        <span>Odometer</span> {{ vehicleData.odo }}
      </div>
    </div>
  </div>
</template>

<script>
import MapContainer from './components/MapContainer.vue'

export default {
  name: 'App',
  components: {
    MapContainer
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
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.map-container {
  height: 500px;
  width: 50%;
}

</style>
