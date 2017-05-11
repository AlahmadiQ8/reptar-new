---
title: Common Algorithms Written in Javascript (ES6)
date: 2017-4-12
draft: false
tags:
  - javascript
  - algorithms
---

I was reviewing Sorting and Search Algorithms and figured I'd implment them all
ES6 Syntax. This will be an open post where I'd regularly update it with more
code snippets.

[[toc]]

## Selection Sort

This is the simplest sorting algorithm. It's good to at least know one or
two sorting algorithms.

```javascript
const swap = (arr, a, b) => {
  const temp = arr[a];
  arr[a] = arr[b];
  arr[b] = temp;
}
const selectionSort = (arr) => {
  let min;

  for (let i=0; i<arr.length; i++ ) {
    min = i;
    for (let j = i+1; j<arr.length; j++) {
      if (arr[j] < arr[min]) {
        min = j;
      }
    }
    if (min !== i) {
      swap(arr, min, i);
    }
  }
}

const arr = [4,1,6,3,8,3,7,2,4];
selectionSort(arr)
console.log(arr); // [ 1, 2, 3, 3, 4, 4, 6, 7, 8 ]
```

## Binary Search

```javascript
const binarySearch = (arr, value) => {
  let low = 0, high = arr.length-1, mid;

  while (low <= high) {
    mid = parseInt(((high + low) / 2));
    if (value < arr[mid]) {
      high = mid-1;
    } else if (value > arr[mid]) {
      low = mid+1;
    } else {
      return mid;
    }
  }

  return null;
}

const binarySearchRecursive = (arr, value, low, high) => {
  if (low > high) return null;
  const mid = parseInt(((high + low) / 2));
  if (value < arr[mid]) {
    return binarySearchRecursive(arr, value, low, mid-1);
  } else if (value > arr[mid]) {
    return binarySearchRecursive(arr, value, mid+1, high);
  } else {
    return mid;
  }
}

const arr = [1,3,5,7,8,10]
console.log(binarySearch(arr, 1));  // should return 0
console.log(binarySearch(arr, 5));  // should return 2
console.log(binarySearch(arr, 10)); // should return 5
console.log(binarySearch(arr, 11)); // should return null
console.log(binarySearchRecursive(arr, 1, 0, arr.length-1));  // should return 0
console.log(binarySearchRecursive(arr, 5, 0, arr.length-1));  // should return 2
console.log(binarySearchRecursive(arr, 10, 0, arr.length-1)); // should return 5
console.log(binarySearchRecursive(arr, 11, 0, arr.length-1)); // should return null
```

## Merge two sorted arrays into one sorted array

```javascript
function merge(arrA, arrB) {
  let i = 0, j = 0;
  const arr = [];

  while( i < arrA.length || j < arrB.length) {
    if (i < arrA.length && arrA[i] <= arrB[j]) {
      arr.push(arrA[i]);
      i += 1;
    } else if (j < arrB.length) {
      arr.push(arrB[j]);
      j += 1;
    }
  }
  return arr;
}

const arrA = [1,3,5,7,8,10];
const arrB = [2,3,6,7,9,11];

console.log(merge(arrA, arrB));  // should be [ 1, 2, 3, 3, 5, 6, 7, 7, 8, 9, 10, 11 ]
```

