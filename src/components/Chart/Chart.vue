<template>
  <div class='chart-container'>
    <canvas ref="chartCanvas" id="tutorial" @mousedown="mouseDown" @mouseenter="mouseEnter" @mouseleave="mouseLeave"
      :width="chartWidth" :height="chartHeight"> </canvas>
  </div>
</template>

<script setup>
// Imports
import { ref, onMounted, defineProps, computed, onDeactivated } from 'vue'

// Props
let { tickData } = defineProps({
  tickData: Array
})

// Variables
let chartId = Math.round(Math.random() * 1000)
let chartCanvas = ref(null)
let ctx
let chartHeight = ref(400)
let chartWidth = ref(800)
let candleWidth = ref(10)
let candleMinHeight = ref(2)
let candleGap = ref(5)

onMounted(() => {
  ctx = chartCanvas.value.getContext('2d')
  setupMouseUp()
  drawChart()
})


// functions
function drawCandle(colour, x, y, width, height) {
  ctx.fillStyle = "rgb(200, 0, 0)";
  ctx.fillRect(x, y, width, height);
}

function drawChart() {
  let numCandles = Math.min(tickData.length, Math.floor(chartWidth.value / (candleWidth.value + candleGap.value)))
  let priceMin = Math.min(...tickData.map(tick => tick.price))
  let priceMax = Math.max(...tickData.map(tick => tick.price))
  let priceRange = priceMax - priceMin
  for (let i = 0; i < numCandles; i++) {
    let x = candleWidth.value + candleGap.value + numCandles * i
    const price = tickData[i].price;
    let y = chartHeight.value * ((price - priceMin) / priceRange)
    drawCandle(null, x, y, candleWidth.value, candleMinHeight.value)
  }
}

computed(() => {

})

const mouseStatuses = {
  UP: 'up',
  DOWN: 'down'
}

let mouseStatus = ref(mouseStatuses.UP)

function mouseupHandler(event) {
  if (mouseStatus.value == (chartId + mouseStatuses.DOWN)) {
    mouseStatus.value = chartId + mouseStatuses.UP
    mouseDragEndPositionX = event.pageX

    console.log('mouse up', chartId)
  }
}

function setupMouseUp() {
  let _mouseupHandler = document.addEventListener('mouseup', mouseupHandler)

  onDeactivated(() => {
    document.removeEventListener(_mouseupHandler)
  })
}

function mouseEnter(event) {
  console.log('enter')
}

function mouseLeave(event) {
  console.log('leave')
}

let mouseDragStartPositionX = 0
let mouseDragEndPositionX = 0
function mouseDown(event) {
  mouseStatus.value = chartId + mouseStatuses.DOWN
  console.log(event)
  mouseDragStartPositionX = event.pageX
}

</script>

<style>
.chart-container {
  width: 800px;
  height: 400px;
}
</style>
