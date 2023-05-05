<template class="flex flex-row">
  <ApexCharts
    @click="clickChart"
    :width="width"
    :height="height"
    :options="options"
    :series="series"
    :type="type"
  />
</template>

<script setup>
import ApexCharts from 'vue3-apexcharts'

const emit = defineEmits(['clickChart'])
const props = defineProps({
  options: {
    type: Object,
    required: true
  },
  series: {
    type: Array,
    default: []
  },
  height: {
    type: [String, Number],
    default: '100%'
  },
  width: {
    type: [String, Number],
    default: '100%'
  },
  type: {
    type: String,
    required: true
  }
})

const clickChart = (event, chartContext, config) => {
  emit('clickChart', { event, chartContext, config })
}
</script>
