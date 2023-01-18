<template>
  <Chart :tickData="tickData" />
  <Chart :tickData="tickData" />
</template>
<script>
import { Chart } from '../../../src/index'

export default {
  name: 'ChartExample',
  components: {
    Chart,
  },
  setup() {
    let now = new Date().valueOf()
    let idx = 0
    let tickData = Array.from({ length: 400 }, () => {
      now += idx
      idx++
      return {
        ts: now,
        price: Math.random() * 100,
        volume: 1000
      }
    })
    return {
      tickData
    }
  }
}
</script>
