<script>
  import { Chart, registerables } from "chart.js";
  Chart.register(...registerables);
  import { nextTick } from "vue";

  export default {
    data() {
      return {
        chartInstance: null,
      };
    },
    props: {
      forecastData: {
        type: Array,
        required: true,
      },
    },
    watch: {
      forecastData: {
        handler() {
          this.renderChart();
        },
        deep: true,
        immediate: true,
      },
    },
    methods: {
      handleResize() {
        if (this.chartInstance) {
          this.chartInstance.resize();
        }
      },
      async renderChart() {
        if (!this.forecastData.length) return;

        // wait for DOM build
        await nextTick();

        //then get canvas
        const canvas = this.$refs.chartCanvas;

        //just in case canvas not found
        if (!canvas) {
          console.error("Canvas not found");
          return;
        }

        //destroy previous canvas
        if (this.chartInstance) {
          this.chartInstance.destroy();
        }

        //render chart
        const labels = this.forecastData.map((entry) => {
          const date = new Date(entry.dt * 1000);
          return `${date.getDate()}.${
            date.getMonth() + 1
          } ${date.getHours()}:00`;
        });
        const temps = this.forecastData.map((entry) => entry.main.temp);
        const ctx = canvas.getContext("2d");
        this.chartInstance = new Chart(ctx, {
          type: "line",
          data: {
            labels: labels,
            datasets: [
              {
                label: "Temperature (°C)",
                data: temps,
                fill: false,
                borderColor: "rgb(42, 123, 155)",
                tension: 0.3,
              },
            ],
          },
          options: {
            responsive: true,
            scales: {
              x: {
                ticks: {
                  maxRotation: 45,
                  minRotation: 45,
                },
              },
            },
          },
        });
      },
    },
    mounted() {
      window.addEventListener("resize", this.handleResize);
    },
    beforeUnmount() {
      window.removeEventListener("resize", this.handleResize);
    },
  };
</script>

<template>
  <div class="chartDiv">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<style scoped>
  .chartDiv {
    padding: 20px;
  }
  canvas {
    width: 80%;
  }
  @media screen and (max-width: 650px) {
    .chartDiv {
      padding: 0;
      margin: 0;
    }
    .canvas {
      width: 100%;
    }
  }
</style>
