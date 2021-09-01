<template>
  <div>
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
            <span>Energy</span> {{ vehicleData.energy }}
          </div>
          <div class="indicator-list__item">
            <span>Odometer</span> {{ vehicleData.odo }}
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
body {
  margin: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #2c3e50;
  min-height: 100vh;
  color: #fff;
  height: 100%;
  padding: 30px;
}

/* TODO remove/refactor css, separate files, vars/root/base styles */
.map-container {
  height: 500px;
  width: 50%;
}

.flex-container {
  display: flex;
  justify-content: space-between;
}

.flex-item {
  flex-basis: 45%;
}

</style>
