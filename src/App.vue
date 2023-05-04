<script setup>
import MyChart from './components/MyChart.vue';
import rawData from '../utils/data.json';
import { onMounted, ref } from 'vue';

console.log(rawData.null);

const loadedData = ref(rawData.null)
const xAxis = ref([])
const rightAnswers = ref([])
const rightAnswerPercentage = ref()

const data = {
  series: [{
    name: 'PRODUCT A',
    data: [44, 55, 41, 67, 22, 43, 21, 49, 25, 33]
  }, {
    name: 'PRODUCT B',
    data: [13, 23, 20, 8, 13, 27, 33, 12, 17, 12]
  }, {
    name: 'PRODUCT C',
    data: [11, 17, 15, 15, 21, 14, 15, 13, 42, 18]
  }],
  chartOptions: {
    chart: {
      type: 'bar',
      height: 350,
      stacked: true,
      stackType: '100%'
    },
    responsive: [{
      breakpoint: 480,
      options: {
        legend: {
          position: 'bottom',
          offsetX: -10,
          offsetY: 0
        }
      }
    }],
    xaxis: {
      categories: xAxis.value
      // categories: ['2011 Q1', '2011 Q2', '2011 Q3', '2011 Q4', '2012 Q1', '2012 Q2',
      //   '2012 Q3', '2012 Q4'
      // ],
    },
    fill: {
      opacity: 1
    },
    legend: {
      position: 'right',
      offsetX: 0,
      offsetY: 50
    },
  },
}
const axisNameMapping = (data) => {
  for (let key in data) {
    // console.log(key);
    xAxis.value.push(data[key].num)
    // if (myObject.hasOwnProperty(key)) {
    //   myObject[key] *= 2;
    // }
  }
}
const calculatingRightWrong = (data) => {
  let totalAnswerCount = 0
  for (let key in data) {
    const results = data[key].answers_histogram
    const correctAnswers = data[key].correct_answers
    let totalCorrectAnswer = 0
    console.log("RESULTS",results);
    console.log("CORRECT ANSWERS", correctAnswers);
    correctAnswers.map((correctAnswer) => {
      totalCorrectAnswer += results[correctAnswer]
      rightAnswers.value.push(totalCorrectAnswer)
    })
    console.log(totalCorrectAnswer);

    // xAxis.value.push(data[key].num)
    // if (myObject.hasOwnProperty(key)) {
    //   myObject[key] *= 2;
    // }
  }
  console.log(rightAnswers.value);
}
onMounted(() => {
  axisNameMapping(loadedData.value)
  calculatingRightWrong(loadedData.value)
})
console.log(xAxis.value);
</script>

<template>
  <h1>BAR CHART {{ xAxis }}</h1>
  <div>
    <MyChart type="bar" height="350" :options="data.chartOptions" :series="data.series"></MyChart>
  </div>
</template>
