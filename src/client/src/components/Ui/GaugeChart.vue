<template>
    <div>
        <h3 class="chart__label">{{ label }}</h3>
        <div class="chart-container">
            <canvas ref="chartContainer"></canvas>
        </div>
    </div>
</template>

<script>
import GaugeChart from "chartjs-gauge";

export default {
  props: {
    label: {
        type: String,
        default: '',
    },
    scaleData: {
      type: Array,
      required: true,
    },
    backgroundColors: {
      type: Array,
      required: true,
    },
    unit: {
      type: String,
      required: false,
    },
  },
  mounted() {
    this.gaugeChart = new GaugeChart(this.$refs.chartContainer, {
      type: 'gauge',
      data: {
        datasets: [
          {
            value: 0,
            minValue: 0,
            data: this.scaleData,
            backgroundColor: this.backgroundColors,
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
            return `${Math.round(value)} ${this.unit}`;
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
    updateValue(newValue) {
      this.gaugeChart.data.datasets[0].value = newValue;
      this.gaugeChart.update();
    },
  },
};
</script>

<style scoped lang="scss">
.chart-container {
    position: relative;
    width: 50%;
    margin: 0 auto var(--space-sm);

    @include lg {
        width: 75%;
        margin: 0;
        margin-bottom: var(--space-lg);
    }
}
</style>
