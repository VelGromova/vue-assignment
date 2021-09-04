<template>
  <canvas ref="chartContainer"></canvas>
</template>

<script>
import GaugeChart from 'chartjs-gauge';

export default {
  mounted() {
    this.gaugeChart = new GaugeChart(this.$refs.chartContainer, {
      type: 'gauge',
      data: {
        datasets: [
          {
            value: 60,
            minValue: 0,
            data: [90, 130],
            backgroundColor: ['blue', 'red'],
          },
        ],
      },
      options: {
        needle: {
          radiusPercentage: 2,
          widthPercentage: 3.2,
          lengthPercentage: 80,
          color: 'rgba(0, 0, 0, 1)',
        },
        valueLabel: {
          display: true,
          fontSize: 18,
          formatter: (value) => {
            return Math.round(value);
          },
          color: 'rgba(255, 255, 255, 1)',
          backgroundColor: 'rgba(0, 0, 0, 1)',
          borderRadius: 5,
          padding: {
            top: 10,
            bottom: 10,
          },
        },
      },
    });
  },

  methods: {
    updateChart(value) {
      this.gaugeChart.data.datasets[0].value = value;
      this.gaugeChart.update();
    },
  },
};
</script>
