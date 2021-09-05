<template>
    <div class="map-container">
        <div id="mapContainer" style="height: 100%" />
    </div>
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
            marker: null,
        }
    },
    watch: {
        coordinates(newCoordinates) {
            var newLatLng = new L.LatLng(newCoordinates.lat, newCoordinates.lng);
            this.map.panTo(newLatLng);
            this.marker.setLatLng(newLatLng); 
        }
    },
    mounted() {
        this.map = L.map('mapContainer').setView(new L.LatLng(this.coordinates.lat, this.coordinates.lng), 20);
        L.tileLayer("http://{s}.tile.osm.org/{z}/{x}/{y}.png", {
        attribution:
            '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(this.map);
        var greenIcon = L.icon({
            iconUrl: require('@/assets/pin-icon.svg'),
            iconSize: [38, 95],
            iconAnchor: [22, 94],
        });
        this.marker = L.marker([this.coordinates.lat, this.coordinates.lng], {icon: greenIcon}).addTo(this.map);
    },
    beforeMount() {
        if (this.map) {
            this.map.remove();
        }
    }
}

</script>

<style lang="scss">
    .map-container {
        height: rem(500);
        width: 100%;
        margin-bottom: var(--space-sm);

        @include lg {
            width: 50%;
        }
    }
</style>