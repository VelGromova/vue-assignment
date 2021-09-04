<template>
  <div class="row">
    <h1>Vehicle data</h1>
    <div class="flex-container">
      <div class="map-container">
        <MapContainer :coordinates="parsedCoordinates"/>
      </div>
      <div class="flex-item">
        <ProgressBar :label="'Current speed'" :value="currentVehicleData.speed" :unit="'km/h'" />
        <ProgressBar :label="'State of charge'" :value="stateOfCharge" :unit="'%'" />
        <div class="indicator-list">
          <div class="indicator-list__item">
            <h3>Energy</h3> <span>{{ currentVehicleData.energy }}</span>
          </div>
          <div class="indicator-list__item">
            <h3>Odometer</h3> <span>{{ currentVehicleData.odo }}</span>
          </div>
        </div>
      </div>
    </div>
    <LineChart ref="lineChart"/>
  </div>
</template>

<script>
import MapContainer from './components/MapContainer.vue';
import ProgressBar from './components/Ui/ProgressBar.vue';
import LineChart from './components/LineChart.vue';

export default {
  name: 'App',
  components: {
    MapContainer,
    ProgressBar,
    LineChart,
  },

  data() {
    return {
      connection: null,
      vehicleData: [],
      currentVehicleData: {
        speed: 0,
        soc: 0,
        time: 0,
      },
    }
  },

  computed: {
    parsedCoordinates() {
      if (this.currentVehicleData.gps) {
        const [lat, lng] = this.currentVehicleData.gps.split('|').map(parseFloat);
        return {lat, lng};
      }
      return { lat: 52.09126663208008, lng: 5.122183322906494};
    },
    stateOfCharge() {
      return Math.floor(this.currentVehicleData.soc);
    },
    convertedTime() {
      return new Date(this.currentVehicleData.time)
    }
  },

  created() {
    this.connection = new WebSocket('ws://localhost:3000/')

    this.connection.onmessage = (event) => {
      this.currentVehicleData = JSON.parse(event.data);
      this.vehicleData.push(this.currentVehicleData);
      this.vehicleData = this.vehicleData.slice(-50);
      const timeData = this.vehicleData.map(() => '');
      const speedData = this.vehicleData.map(vehicle => vehicle.speed);
      this.$refs.lineChart.updateChart(timeData, speedData);
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
}
</style>
