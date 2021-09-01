<template>
  <div id="mapContainer" style="height: 100%" />
</template>

<script> 
import 'leaflet/dist/leaflet.css';
import L from 'leaflet';

export default {
    name: 'MapContainer',
    props: {
        coordinates: {
            type: Object,
            required: true,
        }
    },
    data() {
        return {
            map: null,
        }
    },
    mounted() {
        this.map = L.map('mapContainer').setView(new L.LatLng(this.coordinates.lat, this.coordinates.lng), 20);
        L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
        attribution:
            '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(this.map);
    }
}

</script>