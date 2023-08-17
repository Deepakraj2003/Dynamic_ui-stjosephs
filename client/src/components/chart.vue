<template>
  <div class="chart-container">
    <canvas
      ref="chartCanvas"
      @mousemove="handleMouseMove"
      @mouseleave="handleMouseLeave"
    ></canvas>
    <div
      v-if="showTooltip"
      class="tooltip"
      :style="{
        top: tooltipPosition.top + 'px',
        left: tooltipPosition.left + 'px',
      }"
    >
      {{ tooltipValue }}
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import json from "./char.json";
export default {
  data() {
    return {
      chartData:json.chartdata,
      // chartData: [10,59,35,27,89,64,77,83], // Sample data
      fetchedChartData: [],
      barColor: "#4286f4", // Bar color
      hoverColor: "#ff0000", // Hovered bar color
      barWidth: 40, // Bar width in pixels
      barSpacing: 20, // Spacing between bars in pixels
      canvasHeight: 200, // Height of the canvas
      hoveredBarIndex: null, // Index of the currently hovered bar
      showTooltip: false, // Flag to show/hide the tooltip
      tooltipPosition: { top: 0, left: 0 }, // Position of the tooltip
      tooltipValue: "", // Value to display in the tooltip
    };
  },
  mounted() {
    this.drawChart();
    this.fetchChartData();
  },
  methods: {
    fetchChartData() {
      axios
        .get('http://localhost:3000/chartdata')
        .then((response) => {
          this.fetchedChartData = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },

    drawChart() {
      const canvas = this.$refs.chartCanvas;
      const ctx = canvas.getContext("2d");

      // Calculate the maximum value in the chart data
      const maxValue = Math.max(...this.chartData);

      // Calculate the width of the canvas based on the data
      const canvasWidth =
        this.chartData.length * (this.barWidth + this.barSpacing) -
        this.barSpacing;

      // Set the canvas width and height
      canvas.width = canvasWidth;
      canvas.height = this.canvasHeight;

      // Draw each bar
      this.chartData.forEach((value, index) => {
        const barHeight = (value / maxValue) * this.canvasHeight;

        // Set the bar color
        ctx.fillStyle =
          index === this.hoveredBarIndex ? this.hoverColor : this.barColor;

        // Calculate the position of the current bar
        const x = index * (this.barWidth + this.barSpacing);
        const y = this.canvasHeight - barHeight;

        // Draw the bar
        ctx.fillRect(x, y, this.barWidth, barHeight);
      });
    },
    handleMouseMove(event) {
      const canvas = this.$refs.chartCanvas;
      const rect = canvas.getBoundingClientRect();
      const x = event.clientX - rect.left;

      this.hoveredBarIndex = Math.floor(x / (this.barWidth + this.barSpacing));

      // Update tooltip position and value
      this.tooltipPosition = {
        top: event.clientY - rect.top + 10,
        left: event.clientX - rect.left + 10,
      };
      this.tooltipValue = this.chartData[this.hoveredBarIndex];

      // Show tooltip
      this.showTooltip = true;

      // Redraw the chart with the hover effect and tooltip
      this.drawChart();
    },
    handleMouseLeave() {
      this.hoveredBarIndex = null;
      this.showTooltip = false;

      // Redraw the chart to remove the hover effect and tooltip
      this.drawChart();
    },
  },
};
</script>

<style>
.chart-container {
  position: relative;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

canvas {
  border: 1px solid #ddd;
  background-color: #f9f9f9;
}

canvas:hover {
  cursor: pointer;
}

.tooltip {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 5px 10px;
  font-size: 14px;
  border-radius: 5px;
}
</style>