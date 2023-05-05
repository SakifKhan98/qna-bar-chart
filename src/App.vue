<script setup>
import MyChart from './components/MyChart.vue'
import rawData from '../utils/data.json'
import { ref, watchEffect } from 'vue'

const loadedData = ref(rawData.null)
const mainChartXAxis = ref([])
const rightAnswers = ref([])
const wrongAnswers = ref([])

const subChartSeriesCorrectAnswers = ref([])
const selectedQuestion = ref(null)
const subChartSeries = ref([
  {
    data: []
  }
])

/** Function for formatting raw data loaded from api (in our case mock data file) to make it usable with chart */
const formatMainChartData = (data) => {
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
  }
}
/** Function for formatting the Axes names of the Main Chart (In our case Question Numbers below each bar) */
const formatMainChartAxis = (data) => {
  for (let key in data) {
    mainChartXAxis.value.push(data[key].num)
  }
}
/** Function for handling the event when a user click on any bar in the chart */
const clickChartHandler = ({ event, chartContext, config }) => {
  subChartSeries.value = [
    {
      data: []
    }
  ]
  selectedQuestion.value = config.dataPointIndex + 1
}
/** Function for formatting data for showing in the respective SubChart after clicking on a bar in main Chart */
const formatSubChartData = (selectedQuestionNo) => {
  if (selectedQuestionNo) {
    let subChartAxisData = []
    let selectedQuestionData = loadedData.value[selectedQuestionNo]
    let correctAnswers = selectedQuestionData.correct_answers
    let answersHistogram = selectedQuestionData.answers_histogram

    answersHistogram = Object.fromEntries(
      Object.entries(answersHistogram).sort()
    )
    for (let key in answersHistogram) {
      if (key === 'null') {
        subChartAxisData.push('-')
      } else {
        subChartAxisData.push(key)
      }
      subChartSeries.value[0].data.push(answersHistogram[key])
    }
    subChartSeriesCorrectAnswers.value = correctAnswers
    updateSubChartXAxis(subChartAxisData)
  }
}
/** Function for updating SubChart axis names when clicking from one to another*/
const updateSubChartXAxis = (xAxisValue) => {
  subChartData.value.chartOptions = {
    ...subChartData.value.chartOptions,
    xaxis: {
      categories: xAxisValue
    }
  }
}
/** Chart Options & Other Data for main chart */
const mainChartData = ref({
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
    xaxis: {
      categories: mainChartXAxis.value
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
})
/** Chart Options & Other Data for mSubain chart */
const subChartData = ref({
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
    plotOptions: {
      bar: {
        horizontal: true
      }
    },
    xaxis: {
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
          let seriesIndex = config.seriesIndex
          const totalAnswerCount = config.series[seriesIndex].reduce(
            (a, b) => a + b,
            0
          )
          const percentValue = Math.floor((val / totalAnswerCount) * 100)
          return percentValue + '% ' + 'of total answers'
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
/** Used WatchEffect for formatting data for Main Chart when its loaded  */
watchEffect(() => {
  formatMainChartData(loadedData.value)
  formatMainChartAxis(loadedData.value)
})
/** Used WatchEffect for formatting subchart data when selected question is changed by clicking on bar in main chart  */
watchEffect(() => {
  formatSubChartData(selectedQuestion.value)
})
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
        @clickChart="clickChartHandler"
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
        type="bar"
        height="450"
        width="500"
        :options="subChartData.chartOptions"
        :series="subChartSeries"
      />
    </div>
    <div class="text-2xl text-gray-600 font-bold" v-else>
      Select a question for detailed breakdown...
    </div>
  </div>
</template>
