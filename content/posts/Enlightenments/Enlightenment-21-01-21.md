---
title: "Enlightenment of the Day 21.01.21"
categories: ["Enlightenment"]
tags: ["Javascript", "Problem-Solving", "Frontend", "Array", "max"]
date: 2021-01-21T13:58:10+09:00
draft: false
---

# Getting Student with Highest Score
## Problem

    3 Students are taking a multiple choice test with 5 answer options. Without actually solving the problem, they repeat the same pattern of numbers. When fed with the answer array, return the student who scored the highest.
    
## Requirements
    1. The pattern of each student is as follows:

      Student 1: 1, 2, 3, 4, 5, 1, 2, 3, 4, 5, 1, 2 ....
      Student 2: 2, 1, 2, 3, 2, 4, 2, 5, 2, 1, 2, 3 ....
      Student 3: 3, 3, 1, 1, 2, 2, 4, 4, 5, 5, 3, 3, 1, 1, 2, 2, ....

    2. If there is more than one student scoring the highest score, return all of them in an ascending order.

## My Approach
`Logic:` Generate the array for each student, in the length of the answers array length > Compare each with the answer array, incrementing scores when match > Compare all the scores to identify the high scorer

### 1. Generating the Array for Student 1
``` Javascript
const problem_length = answers.length;
let first =[];
let ele1 = 1;
for(let i = 0; i < problem_length; i++) {
  first.push(ele1)
  if (ele1 < 5){
    ele1++
  } else {
    ele1 = 1
  };
};
```

### 2. Generating the Array for Student 2
``` Javascript
let second = [];
let ele2 = 1;
for(let j = 0; j < problem_length; j++) {
  if (j%2 == 0) {
    second.push(2)
  } else {
    second.push(ele2)
    if (ele2 == 1) {
      ele2 += 2
    } else if (ele2 == 5){
      ele2 = 1
    } else {
      ele2++
    }
  }
}
```

### 3. Generating the Array for Student 3
``` Javascript
let third = [];
let arr3 = [3, 1, 2, 4, 5];
let l = 0;
for(let k = 0; k < problem_length; k++) {
  third[k] = arr3[l];
  if (third[k] == third[k-1]){
    if (l < 4) {
      l++
    } else {
      l = 0;
    }
  }
}
```

### 4. Create Function for Checking Answer
``` Javascript
function check(submit, answers) {
  let score = 0;
  for(let i = 0; i < submit.length; i++) {
    if (submit[i] == answers[i]) {
      score++
    }
  }
  return score
}
```
### 5. Run Check Function, Get Scores
``` Javascript
  const f_score = check(first, answers);
  const s_score = check(second, answers);
  const t_score = check(third, answers);
```

### 6. Compare Scores, Find out High Score
``` Javascript
let answer = [];
if (f_score > s_score) {
  if (f_score > t_score) {
    answer.push(1)
  } else if (f_score < t_score) {
    answer.push(3)
  } else {
    answer.push(1)
    answer.push(3)
  }
} else if (f_score < s_score) {
  if (s_score > t_score) {
    answer.push(2)
  } else if (s_score < t_score) {
    answer.push(3)
  } else {
    answer.push(2)
    answer.push(3)
  }
} else {
  if (f_score == t_score) {
    answer.push(1)
    answer.push(2)
    answer.push(3)
  } else {
    answer.push(1)
    answer.push(2)
  }
}
```

## An Ideal Solution Someone Else Proposed
`Logic:` Create an Array, consisting of each student's answer patterns > Loop through the answers array > In each iteration, compare the answers with what each student's would answer (without creating the actual array) > Increment scores when correct > Get max "score" > Push student to result array, when the student's score is the high score

This Solution is way more compact, efficient and self-explanatory. Although I also thought of a similar solution, I couldn't go along with that, because I did not know how I would loop repeatedly through a specific length of pattern, an unknown amount of times. The answer was rather simple.

```Javascript
const man1 = [1, 2, 3, 4, 5];
const man2 = [2, 1, 2, 3, 2, 4, 2, 5];
const man3 = [3, 3, 1, 1, 2, 2, 4, 4, 5, 5];
let count = [0, 0, 0];

for(let i = 0; i < answers.length; i++) {
    if(answers[i] == man1[i % man1.length]) count[0]++;
    if(answers[i] == man2[i % man2.length]) count[1]++;
    if(answers[i] == man3[i % man3.length]) count[2]++;
}
```
`i`(iteration) modulo length of pattern array, meaning the remainder of dividing the iteration by the length of the pattern, would effetively choose the exact value in the pattern as desired. This would also directly be compared with the answer array, and when the answer is correct, score count is incremented.

Getting the high score and the final result array could also be very efficient using the Math.max method.

``` Javascript
const max = Math.max(count[0], count[1], count[2]);
for(let i = 0; i < count.length; i++) {
    if(max == count[i]) answer.push(i + 1);
}
```
When the max number of the score count can be found, this can be compared to each of the scores and the score's index plus one is the student number, which can be pushed to the answer array, when high score.




____
I should probably work on my math.
