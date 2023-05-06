# Project Title

### QnA Bar Chart

## Project Description

This is a simple front-end for a bar chart visualizing responses of students to a series of questions. It also demonstrates a breakdown of each questions using a different instance of the same chart component. It is build using `Vue3`, `ApexCharts`, `TailwindCSS`, and `Vite`.

## Project Live Link:

[Live Link - QnA Bar Chart](https://qna-bar-chart.netlify.app/)

## Demo Video Link:

[Demo video - QnA Bar Chart](https://drive.google.com/file/d/1sWR_9YlGwr7DbU37r7bdZ9w2y57Btbah/view?usp=share_link)

## Getting Started

### Cloning Repository

(Ask for permission if you don't have -- It is a private repo)

```sh
git clone https://github.com/SakifKhan98/qna-bar-chart.git
```

### Installing Dependencies

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

You can check the features of the bar chart as shown in the above mentioned demo video.
<br />
<br />

# Features:

1. Data is loaded from a separate file stored in the `utils` folder.
2. ApexChartJS is used to visualize the data in the form of bar chart.
3. User can hover over the bar to see the percentage of students who have given the right & wrong answer for a particular question (Right Answer - Green Portion, Wrong Answer - Red Portion)
4. User can click on a bar to see further breakdown of that question's response on the right side of the app.
5. If a user clicks on a question (bar in the graph), the breakdown chart will show the response of that question only. This breakdown will show how many students responsed to which option and how many of them were right/wrong.
6. When hovered over a option (bar in Chart) in the breakdown chart, the user can see the percentage of students who have given that option as their answer.
7. User can interactively select any question in the main chart and the breakdown chart will update automatically.

<br />

# Further Improvements:

We can improve the app in several ways. Some of them are:

### Improvements in the UI/UX:

1. We can add some animation or delay to the chart to make it more interactive.
2. We can use different colors for better visualization.
3. We can add Legends for better understanding of the chart.
4. We can add more features to the chart like zooming, panning, etc.
5. We can add option for downloading the Chart as SVG/PNG/JPEG for further use.

### Improvements in the Code Level:

1. The code became a bit verbose due to the nature of Charting Libraries. We can refactor the code to make it more readable and maintainable.
2. We can use `computed` property of Vue to do the calculations for better caching and performance.
3. We can breakdown the functions in the `App.vue` file to different (at least two - one for main chart, another for the breakdown chart) `composable` files to make the code more modular and re-usable.
4. We can add Linter & Prettier for better code organization.
