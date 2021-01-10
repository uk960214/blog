---
title: "Enlightenment of the Day 21.01.11"
date: 2021-01-11T00:07:50+09:00
draft: false
---
# Situation
## Solving the following task on Programmers ([Link](https://programmers.co.kr/learn/courses/30/lessons/68644))

### Task
>An interger array 'numbers' is given. Create a function that returns an array consisting of sums of 2 numbers of different index sorted in an ascending order.

### Resitrictions
>* The length of 'numbers' is between 2 to 100.
>* All of the numbers of are bewtween 0 to 100.

### Input/Output example
>|numbers|result|
>|:-------:|:------:|
>|[2, 1, 3, 4, 1]|[2, 3, 4, 5, 6, 7]|
>|[5, 0, 2, 7]|[2, 5, 7, 9, 12]|

<br>

# My Approach to the Solution
* ## Initially

> **Logic** 
<br>
 `Sort the input array in ascending order` => `loop through the array using forEach` => `in each iteration, map the array, adding the current value to the array` => `add all that to a result array` => `using Set, pick out unique values`

Initially at this point, I thought using the map method and using Set to create an array of unique values was the crucial point of the solution.

However, even after it passed the example inputs of the above, during the real evaluation, 7 out of 9 test cases turned out to be a failure.

My prediction of the point of error was merging of the arrays. So instead of  concatenating using `Array.concat()`, I turned to `Array.prototype.push.apply()`, which again, however, turned out the same results.

Next prediction was that I had to much of the same sums, that somewhere in between there must be an error. I tried to splicing and slicing the existing array to avoid adding the same numbers again. But yet another failure.

* ## After couple failures
At this point, I thought it had to be the map method, which wasn't working for me in the way I wanted to. So I simply turned to using nested for loop. 

> **Logic** 
<br>
 `Sort the input array in ascending order` => `loop through the array using for` => `in each iteration, loop again using for and add the current value to all the following elements` => `add all that to a result array` => `using Set, pick out unique values`

But (surprise, surprise!) it DIDN'T WORK.

<br>

# The Actual Problem

Since all of my other attempts failed tragically, the only thing left to be altered was the sort. I thought that maybe sorting the array in the beginning is a bad idea. Maybe, I thought, if the array was sorted in the end, it won't be messed up.
> **Logic**
 <br> 
 `loop through the array using for` => `in each iteration, loop again using for and add the current value to all the following elements` => `add all that to a result array` => `using Set, pick out unique values` => ***`Sort the input array in ascending order`***

This also didn't turn out as I expected, but something stranged showed up while checking the example inputs. '12' was positioned in the beginning of the array, instead of at the very end.

<br>

<div align=center>I DID NOT SPECIFY THE COMPARE FUNCTION IN THE SORT METHOD</div>

<br>

So, although I might have had some problems, I could have solved the task way quicker, 
if I only had put in the compare function in the sort method.

## **Lesson learnt, the hard way.**

<br>

# Answer Code

```javascript
function solution(numbers) {
  let result = []

  for(let i = 0; i < numbers.length; i++) {
    for(let j = i+1; j < numbers.length; j++) {
      result.push(numbers[i]+numbers[j])
    }
  }

  const answer = Array.from(new Set(result)).sort((a,b) => a-b)
  return answer
}
```

# Still a Mystery
Why does the nested for loop evaluated as a high score, instead of the map method?