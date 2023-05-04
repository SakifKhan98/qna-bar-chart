<template>
  <ApexCharts
    :width="width"
    :height="height"
    :options="options"
    :series="series" />
</template>

<script setup>
import ApexCharts from 'vue3-apexcharts'

defineProps({
  options: {
    type: Object,
    required: true
  },
  series: {
    type: Array,
    default: () => ([])
  },
  height: {
    type: [String, Number],
    default: '100%'
  },
  width: {
    type: [String, Number],
    default: '100%'
  }
})
</script>
