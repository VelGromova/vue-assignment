<template>
    <canvas ref="chartContainer"></canvas>
</template>

<script>
import { Chart, registerables } from 'chart.js';
import 'chartjs-adapter-date-fns';
Chart.register(...registerables);

export default {
    props: {
        xAxisTitle: {
            type: String,
            required: false,
        },
        yAxisTitle: {
            type: String,
            required: false,
        },
        min: {
            type: Number,
            required: false,
        },
        max: {
            type: Number,
            required: false,
        },
        xAxisTimeUnit: {
            type: String,
            required: false,
        }
    },

    mounted() {
        this.lineChart = new Chart(this.$refs.chartContainer, {
            type: 'line',
            data: {
                labels: [],
                datasets: [{
                    data: [],
                    pointRadius: 0,
                    borderColor: 'rgb(75, 192, 192)',
                    tension: 0.1
                }]
            },
            options: {
                plugins: {
                    legend: {
                        display: false,
                        labels: {
                            fontColor: 'white',
                        }
                    },
                },
                scales: {
                    x: {
                        type: this.xAxisTimeUnit ? 'time' : undefined,
                        time: {
                            unit: this.xAxisTimeUnit,
                        },
                        title: {
                            display: true,
                            text: this.xAxisTitle,
                            color: '#fff',
                        },
                        ticks: {
                            color: '#fff',
                        },
                        grid: {
                            color: 'rgba(255, 255, 255, 0.1)',
                        }
                    },
                    y: {
                        title: {
                            display: true,
                            text: this.yAxisTitle,
                            color: '#fff',
                        },
                        ticks: {
                            color: '#fff',
                        },
                        min: this.min,
                        max: this.max,
                        grid: {
                            color: 'rgba(255, 255, 255, 0.1)',
                        }
                    },
                }
            }
        });
    },

    methods: {
        updateChart(labels, dataValues) {
            this.lineChart.data.datasets[0].data = dataValues;
            this.lineChart.data.labels = labels;
            this.lineChart.update();
        },
    },
}
</script>