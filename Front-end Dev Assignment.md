
# Summary
Make a simple web front end for drawing a chart using vuejs.

# Description
We need to show a chart on a website. The data for this chart is given with this assignment. We need you to write a web front end using vuejs for showing this chart. Using vue3, write an app and a component for showing the chart. A video is also attached to show the desired interactions. Your chart does not have to match the exact styling from the video. In fact, if you use a modern charting library and create pleasant looking charts, we might throw away the old ones to use your creation.

## Chart specifications
- Each bar in the chart represents one question. In the data, `"num": 1` means the 1st question. Data for the first bar is found in the object that has num:1, data for the second question is in the object that has num:2 and so on.
- Every bar in the chart is same height.
- Bars are divided into two parts: green part represents proportion of correct answers, red part represents wrong answrs.
- Every bar in the bar chart is interactive.
- Click on a bar to display further breakdown of data on the right hand side.
- The barch chart on right hand side shows histogram of all answers.
- Hovering over a bar in the histogram should display the percentage for that answer.

## Requirements
- Use vue3.
- Write a vue app and a vue component for showing the chart.
- Pay close attention to the interactions in the video.

## Delivery
- Createa a repository in github.
- Invite me to view your code after completion.

## Data
```
{"null": {"1": {"num": 1, "answers_histogram": {"null": 0, "A": 14, "D": 1, "C": 2, "E": 1, "B": 1}, "correct_answers": ["A", "B"]}, "2": {"num": 2, "answers_histogram": {"null": 0, "A": 5, "D": 0, "C": 0, "E": 1, "BC": 1, "AB": 1, "B": 11}, "correct_answers": ["A", "B"]}, "3": {"num": 3, "answers_histogram": {"null": 0, "A": 1, "D": 2, "C": 1, "E": 0, "B": 15}, "correct_answers": ["B"]}, "4": {"num": 4, "answers_histogram": {"null": 0, "A": 15, "D": 0, "C": 2, "E": 0, "B": 2}, "correct_answers": ["A"]}, "5": {"num": 5, "answers_histogram": {"null": 0, "A": 0, "D": 1, "C": 14, "E": 2, "B": 2}, "correct_answers": ["C"]}, "6": {"num": 6, "answers_histogram": {"null": 0, "A": 0, "D": 13, "C": 5, "E": 1, "B": 0}, "correct_answers": ["D"]}, "7": {"num": 7, "answers_histogram": {"null": 0, "A": 12, "D": 2, "C": 2, "E": 2, "B": 1}, "correct_answers": ["A"]}, "8": {"num": 8, "answers_histogram": {"null": 0, "A": 3, "D": 1, "C": 1, "E": 12, "B": 2}, "correct_answers": ["E"]}, "9": {"num": 9, "answers_histogram": {"null": 0, "A": 1, "D": 4, "C": 13, "E": 0, "B": 1}, "correct_answers": ["C"]}, "10": {"num": 10, "answers_histogram": {"null": 0, "A": 0, "D": 15, "C": 2, "E": 1, "B": 1}, "correct_answers": ["D"]}}}
```