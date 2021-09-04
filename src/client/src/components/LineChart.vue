<template>
    <canvas ref="chartContainer"></canvas>
</template>

<script>
import { Chart, registerables } from 'chart.js';
Chart.register(...registerables);

let lineChart = null;

export default {
    data() {
        return {
            chartData: {
                labels: [],
                datasets: [{
                    label: 'Speed',
                    data: [],
                    pointRadius: 0,
                    fill: false,
                    borderColor: 'rgb(75, 192, 192)',
                    tension: 0.1
                }]
            }
        }
    },

    mounted() {
        lineChart = new Chart(this.$refs.chartContainer, {
            type: 'line',
            data: this.chartData,
            options: {
                plugins: {
                    legend: {
                        display: false
                    },
                },
                scales: {
                    y: {
                        min: 0,
                        max: 130,
                    }
                }
            }
        });
    },

    methods: {
        updateChart(labels, dataValues) {
            lineChart.data.datasets[0].data = dataValues;
            lineChart.data.labels = labels;
            lineChart.update();
        },
    },
}
</script>
