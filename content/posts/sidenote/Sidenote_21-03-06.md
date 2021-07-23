---
title: "[Sidenote/JS] Using Recursion to Get All Possible Combinations"
categories: ["Sidenote"]
date: 2021-03-06T21:33:37+09:00
draft: false
tags: ["JS", "Problem-Solving", "Algorithm", "Combination", "recursion"]
---

The definition of combination in mathematics is as follows:

> In mathematics, a combination is a selection of items from a collection, such that the order of selection does not matter. (from [Wikipedia](https://en.wikipedia.org/wiki/Combination))

In order to get all the possible selections, the following formula is used:

![combination](https://t1.daumcdn.net/cfile/tistory/99D63E3359A3623122) 

[image-source](https://nackwon.tistory.com/55)

The first part refers to all the combination that includes a certain value, and the latter part refers to all the combinations that does not include that value. Using this notion, it is possible to create a recursive function that figures out all the possible combinations.

## Code 
```js

let combinations = [];


const combination = (array, target, n, r, count) => {
    if(r == 0) {
        combination.push(target)
    } else if(n == 0 || n < r) return;
    else {
        combination(array, Object.assign([], target), n - 1, r, count + 1);
        target.push(array[count]);
        combination(array, Object.assign([], target), n - 1, r - 1, count + 1);
    }
}

combination(indexArray, [], n, k, 0)

}
```

## Explained
---
### Variables

> `combination`: A result array for all the combinations to be pushed to

> `array`: The collection in array form

> `target`: The working array that consists of the elements that is currently being processed

> `n`: Number of elements in the collection

> `r`: Number of selections desired

> `count`: Counting index to target the next element in the collection as the recursion repeats


### Base Case
```JS
else if(n == 0 || n < r) return;
```
The base case consists of two conditions. THe first one is when there isn't any collection left to select from. The second one is when n is smaller than r, meaning that the number that needs to be selected exceeds the number of collection to select from. In these two cases, the function returns.

### Recursion
As explained above, the formula for getting combination consists of two parts, which can be expressed in javascript as follows.
```JS
target.push(array[count]);
combination(array, Object.assign([], target), n - 1, r - 1, count + 1);
target.pop();
combination(array, Object.assign([], target), n - 1, r, count + 1);
```
The first two line refers to the former part of the equation, and the next lines the latter. This can be, however, considering that the to parts can changing places, simplified as follows.

```JS
combination(array, Object.assign([], target), n - 1, r, count + 1);
target.push(array[count]);
combination(array, Object.assign([], target), n - 1, r - 1, count + 1);
```

### Adding Full Size Combination to Result Array
When the value of r reaches 0, meaning that there is no need to select anymore, the current working combination, the target, is complete. This is therefore pushed to the result array.

```JS
if(r == 0) {
    combination.push(target)
}
```

[Based on Explantion and Codes of the Following Blog - [Link](https://kjwsx23.tistory.com/366)]
