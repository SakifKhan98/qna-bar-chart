<script setup>
import MyChart from './components/MyChart.vue'
import rawData from '../utils/data.json'
import { onMounted, ref, watchEffect } from 'vue'

// console.log('Main Data', rawData.null)

const loadedData = ref(rawData.null)
const xAxis = ref([])
const rightAnswers = ref([])
const wrongAnswers = ref([])

const subChartSeries = ref([])
const subChartSeriesCorrectAnswers = ref([])
const selectedQuestion = ref(null)
const subChartXAxis = ref([])
const subChartAnswerPercentage = ref(null)
const seriesTT = ref([{ name: 'Answers', data: [] }])
const series = ref([
  {
    data: []
  }
])

const mouseMoveHandler = (event, chartContext, config) => {
  // console.log(config)
  let seriesIndex = config.seriesIndex
  let dataPointIndex = config.dataPointIndex
  // console.log(seriesIndex, dataPointIndex)

  if (seriesIndex >= 0 && dataPointIndex >= 0) {
    let yValue = config.config.series[config.seriesIndex].data[
      config.dataPointIndex
    ].toLocaleString(undefined, { maximumFractionDigits: 2 })
    // let xValue = chartContext.w.globals.labels[config.dataPointIndex]
    const totalAnswerCount = config.config.series[
      config.seriesIndex
    ].data.reduce((a, b) => a + b, 0)
    // console.log(Math.floor((yValue / totalAnswerCount) * 100))
    const percentValue = Math.floor((yValue / totalAnswerCount) * 100)
    // subChartAnswerPercentage.value = Math.floor(
    //   (yValue / totalAnswerCount) * 100
    // )
    // console.log(yValue, xValue)
    // chartYValue.value = yValue
    // chartXValue.value = xValue
    return percentValue
  }
  // else {
  //   chartYValue.value = lastYValue
  //   chartXValue.value = lastXValue
  // }
}

const updateXAxis = (xAxisValue) => {
  subChartData.value.chartOptions = {
    ...subChartData.value.chartOptions,
    xaxis: {
      categories: xAxisValue
    }
  }
}
const formatSubChartData = (selectedQuestionNo) => {
  if (selectedQuestionNo) {
    let subChartAxisData = []
    const selectedQuestionData = loadedData.value[selectedQuestionNo]
    // console.log(selectedQuestionData)
    const correctAnswers = selectedQuestionData.correct_answers
    const answersHistogram = selectedQuestionData.answers_histogram
    // console.log(correctAnswers, answersHistogram)
    // subChartSeries.value = []
    // subChartXAxis.value = []
    for (let key in answersHistogram) {
      // console.log(key.toString())
      subChartAxisData.push(key)
      // subChartSeries.value.push(answersHistogram[key])
      series.value[0].data.push(answersHistogram[key])
    }
    // subChartSeries.value = Object.values(answersHistogram)
    // console.log('NEW ARRAY', subChartSeriesData)
    subChartSeriesCorrectAnswers.value = correctAnswers
    updateXAxis(subChartAxisData)
  }
}

// const updateSubChart = (data) => {}

const clickChart = ({ event, chartContext, config }) => {
  series.value = [
    {
      data: []
    }
  ]
  // subChartSeriesCorrectAnswers.value = []
  // selectedQuestion.value = null
  // subChartXAxis.value = []
  selectedQuestion.value = config.dataPointIndex + 1
  // const dp = config.dataPointIndex
  // console.log('DPPPDPDPDPDP', chartContext.data.twoDSeriesX[dp])
  // console.log('DPPPDPDPDPDP', chartContext.data)

  // const point = event.target.dataset.point
  // if (point !== undefined) {
  //   activeInsights.value = []
  //   const annotationCircleEl = event.target.previousSibling.previousSibling
  //   clearAnnotations()
  //   annotationCircleEl.classList.add('is-active')
  //   const insightSet = props.insights.find((insight) => insight.date === point)
  //   activeInsights.value = insightSet.insights
  //   pointData.value = point
  //   showAnnotation.value = true
  // }
}
const mainChartData = {
  series: [
    {
      name: 'Right Answers',
      data: rightAnswers.value
    },
    {
      name: 'Wrong Answers',
      data: wrongAnswers.value
    }
  ],
  chartOptions: {
    chart: {
      type: 'bar',
      height: 350,
      stacked: true,
      stackType: '100%',
      toolbar: {
        show: false
      }
    },
    // events: {
    //   click: function (event, chartContext, config) {
    //     // The last parameter config contains additional information like `seriesIndex` and `dataPointIndex` for cartesian charts
    //     console.log(event)
    //   }
    // },
    // responsive: [
    //   {
    //     breakpoint: 480,
    //     options: {
    //       legend: {
    //         position: 'bottom',
    //         offsetX: -10,
    //         offsetY: 0
    //       }
    //     }
    //   }
    // ],
    xaxis: {
      categories: xAxis.value
    },
    tooltip: {
      x: {
        formatter: function (value) {
          return 'Question No: ' + value
        }
      }
    },
    fill: {
      opacity: 1,
      colors: ['#0bb236', '#e81717']
    },
    legend: {
      show: false
    }
  }
}

const subChartData = ref({
  // series: [
  //   {
  //     name: 'Answers',
  //     data: subChartSeries.value
  //   }
  // ],
  chartOptions: {
    chart: {
      type: 'bar',
      height: 350,
      stacked: true,
      // stackType: '100%',
      toolbar: {
        show: false
      }
    },
    // states: {
    //   normal: {
    //     filter: {
    //       type: 'none',
    //       value: 0
    //     }
    //   },
    //   hover: {
    //     filter: {
    //       type: 'lighten',
    //       value: 0.15
    //     }
    //   },
    //   active: {
    //     allowMultipleDataPointsSelection: false,
    //     filter: {
    //       type: 'darken',
    //       value: 0.35
    //     }
    //   }
    // },

    plotOptions: {
      bar: {
        horizontal: true,
        dataLabels: {
          total: {
            enabled: false,
            offsetX: 0,
            style: {
              fontSize: '13px',
              fontWeight: 900
            },
            formatter: function (val, context, opt) {
              // let value = mouseMoveHandler(opt.config)
              // console.log(val, context, opt)
              // console.log(context)
              return val + '%'
            }
          }
        }
      }
    },
    xaxis: {
      // categories: subChartXAxis.value
      categories: []
    },
    tooltip: {
      x: {
        formatter: function (value) {
          return 'Option: ' + value
        }
      },
      y: {
        formatter: function (val, config) {
          console.log(val, config)
          let seriesIndex = config.seriesIndex
          // let dataPointIndex = config.dataPointIndex
          // console.log(config.series[seriesIndex][dataPointIndex])
          const totalAnswerCount = config.series[seriesIndex].reduce(
            (a, b) => a + b,
            0
          )
          const percentValue = Math.floor((val / totalAnswerCount) * 100)
          console.log(percentValue)
          return percentValue + '% ' + 'of total answers'

          // if (seriesIndex >= 0 && dataPointIndex >= 0) {
          //   let yValue = config.series[seriesIndex].data[
          //     dataPointIndex
          //   ].toLocaleString(undefined, { maximumFractionDigits: 2 })
          //   // let xValue = chartContext.w.globals.labels[config.dataPointIndex]
          // const totalAnswerCount = config.series[seriesIndex].data.reduce(
          //   (a, b) => a + b,
          //   0
          // )
          //   // console.log(Math.floor((yValue / totalAnswerCount) * 100))
          //   const percentValue = Math.floor((yValue / totalAnswerCount) * 100)
          //   // subChartAnswerPercentage.value = Math.floor(
          //   //   (yValue / totalAnswerCount) * 100
          //   // )
          //   // console.log(yValue, xValue)
          //   // chartYValue.value = yValue
          //   // chartXValue.value = xValue
          //   return percentValue
          // }

          // console.log(seriesIndex, dataPointIndex)
        },
        title: {
          formatter: (val, context, opt) => {
            return null
          }
        }
      }
    },
    fill: {
      opacity: 1,
      colors: [
        function ({ value, seriesIndex, dataPointIndex, w }) {
          const xAxisLabels = w.config.xaxis.categories
          if (
            subChartSeriesCorrectAnswers.value.includes(
              xAxisLabels[dataPointIndex]
            )
          ) {
            return '#0bb236'
          } else {
            return '#e81717'
          }
        }
      ]
    },
    legend: {
      show: false
    }
  }
})
const axisNameMapping = (data) => {
  for (let key in data) {
    // console.log(key);
    xAxis.value.push(data[key].num)
    // if (myObject.hasOwnProperty(key)) {
    //   myObject[key] *= 2;
    // }
  }
}
const extractingData = (data) => {
  for (let key in data) {
    let totalAnswerCount = 0
    const results = data[key].answers_histogram
    const correctAnswers = data[key].correct_answers
    let totalCorrectAnswer = 0
    for (let resultKey in results) {
      totalAnswerCount += results[resultKey]
    }
    correctAnswers.map((correctAnswer) => {
      totalCorrectAnswer += results[correctAnswer]
    })
    rightAnswers.value.push(totalCorrectAnswer)
    wrongAnswers.value.push(totalAnswerCount - totalCorrectAnswer)
    // console.log('Total Answer Count ==>', totalAnswerCount)
    // console.log('Total Correct Answer Count ==>', totalCorrectAnswer)
  }
}

watchEffect(() => {
  axisNameMapping(loadedData.value)
  extractingData(loadedData.value)
})
watchEffect(() => {
  // console.log('Selected Question Value ==>', selectedQuestion.value)
  // console.log('FINAL SUB CHART SERIES', series.value)
  // console.log('Subchart X Axis Value ==> ', subChartXAxis.value)
  formatSubChartData(selectedQuestion.value)
})
onMounted(() => {
  // axisNameMapping(loadedData.value)
  // extractingData(loadedData.value)
  // formatSubChartData(selectedQuestion.value)
})
// console.log(rightAnswers.value.length)
// console.log(wrongAnswers.value.length)
</script>

<template>
  <h1
    class="bg-gray-600 font-bold text-white text-3xl text-center h-20 flex items-center justify-center mb-5"
  >
    QnA BAR CHART
  </h1>
  <div class="flex flex-row gap-5 p-8">
    <div class="pr-6 border-r border-gray-400">
      <MyChart
        type="bar"
        @clickChart="clickChart"
        height="450"
        width="500"
        :options="mainChartData.chartOptions"
        :series="mainChartData.series"
      />
    </div>
    <div v-if="selectedQuestion">
      <h1 class="text-2xl font-bold text-gray-600">
        Question {{ selectedQuestion }} : Breakdown
      </h1>
      <MyChart
        @mouseMove="mouseMoveHandler"
        type="bar"
        height="450"
        width="500"
        :options="subChartData.chartOptions"
        :series="series"
      />
    </div>
    <div class="text-2xl text-gray-600 font-bold" v-else>
      Select a question for detailed breakdown...
    </div>
  </div>
</template>
