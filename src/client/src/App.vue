<template>
  <div class="row">
    <h1>Vehicle data</h1>
    <div class="flex-container"> 
      <MapContainer :coordinates="parsedCoordinates"/>
      <div class="flex-item">
        <GaugeChart :scale-data="[90, 130]" 
                    :background-colors="['blue', 'red']" 
                    unit="km/h" 
                    label="Current speed"
                    ref="speedGaugeChart"/>
        <ProgressBar label="State of charge" 
                    :value="stateOfCharge" 
                    unit="%" />
        <div class="indicator-list">
          <div class="indicator-list__item">
            <h3>Energy</h3> <span>{{ currentVehicleEntry.energy }}</span>
          </div>
          <div class="indicator-list__item">
            <h3>Odometer</h3> <span>{{ currentVehicleEntry.odo }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="flex-container">
      <div class="flex-item">
        <LineChart ref="speedLineChart" y-axis-title="Speed (km/h)" x-axis-title="Time" x-axis-time-unit="second" :min="0" :max="130" />
      </div>
      <div class="flex-item">
        <LineChart ref="socLineChart" y-axis-title="SoC (%)" x-axis-title="Time" x-axis-time-unit="minute" :min="0" :max="100" />
      </div>
    </div>
  </div>
</template>

<script>
import MapContainer from './components/MapContainer.vue';
import ProgressBar from './components/Ui/ProgressBar.vue';
import LineChart from './components/Ui/LineChart.vue'
import GaugeChart from './components/Ui/GaugeChart.vue'

export default {
  name: 'App',
  components: {
    MapContainer,
    ProgressBar,
    LineChart,
    GaugeChart,
  },

  data() {
    return {
      connection: null,
      realTimeVehicleData: [],
      currentVehicleEntry: {
        speed: 0,
        soc: 0,
        time: 0,
      },
      socHistory: [],
    }
  },

  watch: {
    'currentVehicleEntry.soc': function(newSoc, oldSoc) {
      if (newSoc !== oldSoc) {
        this.socHistory.push(this.currentVehicleEntry);
        this.updateSocLineChart();
      }
    }
  },

  computed: {
    parsedCoordinates() {
      const startingPoint = { lat: 52.09126663208008, lng: 5.122183322906494 };
      if (this.currentVehicleEntry.gps) {
        const [lat, lng] = this.currentVehicleEntry.gps.split('|').map(parseFloat);
        return {lat, lng};
      }
      return startingPoint;
    },
    stateOfCharge() {
      return Math.floor(this.currentVehicleEntry.soc);
    },
    convertedTime() {
      return new Date(this.currentVehicleEntry.time)
    }
  },

  created() {
    this.connection = new WebSocket('ws://localhost:3000/')
    this.connection.onmessage = this.onVehicleDataEntry;
  },

  methods: {
    onVehicleDataEntry(event) {
      this.currentVehicleEntry = JSON.parse(event.data);

      // to not overflow the vehicle data array and keep it "real-time"
      const maxVehicleLength = 100;
      if (this.realTimeVehicleData.length >= maxVehicleLength) {
        // shift removes first item, so array always contains max. 300 items
        this.realTimeVehicleData.shift();
      }
      // save vehicle entry
      this.realTimeVehicleData.push(this.currentVehicleEntry);

      // update charts
      this.updateSpeedGaugeChart();
      this.updateSpeedLineChart();
    },
    updateSpeedLineChart() {
      const speedData = this.realTimeVehicleData.map(vehicle => vehicle.speed);
      const speedTimeData = this.realTimeVehicleData.map(vehicle => new Date(vehicle.time));
      this.$refs.speedLineChart.updateChart(speedTimeData, speedData);
    },
    updateSocLineChart() {
      const socData = this.socHistory.map(vehicle => Math.floor(vehicle.soc));
      const socTimeData = this.socHistory.map(vehicle => new Date(vehicle.time));
      this.$refs.socLineChart.updateChart(socTimeData, socData);
    },
    updateSpeedGaugeChart() {
      this.$refs.speedGaugeChart.updateValue(this.currentVehicleEntry.speed);
    },
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
