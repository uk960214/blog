---
title: "[Sidenote/JS] Using Stack for Maximum Efficiency"
categories: ["Sidenote"]
date: 2021-06-28T22:57:42+09:00
draft: false
tags: ["JS", "Algorithm", "Stack", "Time Complexity"]
---

**Solution Reference:**

https://programmers.co.kr/questions/17409

# Task
https://programmers.co.kr/learn/courses/30/lessons/12973

Given a string of characters, if two of the same alphabet in a row is to be deleted until there is none left, check if this possible with the given string.

# Solution
``` JS
function solution(s)
{
    // Split string into array
    let arr = s.split('');
    // Set temporary stack array
    let temp = [];
    
    // If string length is odd number, return 0
    if (arr.length % 2 != 0)
        return 0;

    for (let i = 0; i < arr.length; i++) {
        // If the current char matches the latest item of stack, pop last item
        // If not, push current item to stack
        if(arr[i] === temp[temp.length - 1]) {
            temp.pop();
        } else {
            temp.push(arr[i]);
        }

        // If the amount of char left is smaller than stack, return 0
        if(temp.length > arr.length - i)
            return 0;
    }

    return temp.length ? 0 : 1;
}
```

## Initial Approach
``` JS
function solution(s)
{
    // Split string into array
    let arr = s.split('');
    // Switch to Check if deletion took place
    let del = 0;
    
    // Loop until no deletion happens
    do {
        del = 0;
        for (let i = 0; i < arr.length - 1; i++) {
            // If consequent, delete from array, switch, break loop
            if(arr[i] === arr [i + 1]) {
                arr.splice(i,2);
                del = 1;
                break;
            }
        }
    } while (del)
    

    return arr.length ? 0 : 1;
}
```

Even though my initial approach was working, it was massively inefficient and failed the efficiency test. Probably the complexity was O(n^2). In the provided solution, there are multiple strategies to maximize efficiency.

## 1. Use Stack
My approach ran the array over and over to check if there was anything to delete. However, when using stack, the loop only needs to take place once, since at the end of the stack, the next erasable item is placed, and each of them is compared to the current value. This changes the time complexity to O(n).

``` JS
if(arr[i] === temp[temp.length - 1]) {
    temp.pop();
} else {
    temp.push(arr[i]);
}
```

## 2. Rule out any string with length that is not even
If the length of the string is not even, not every character can be matched to pairs, and therefore, even before beginning the loop, the function can return 0.

```JS
if (arr.length % 2 != 0)
    return 0;
```

## 3. If there aren't enough characters to match those in the stack, return 0
As the title says, the remaining characters of the string should be more than the characters in the string.

```JS
if(temp.length > arr.length - i)
    return 0;
```