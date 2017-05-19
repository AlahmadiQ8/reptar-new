---
title: Testing Time Sensitive Operations with Jest Timer Mocks
date: 2017-05-13
draft: false
disqus_unique: 2017-05-13-testing-with-timer-mocks
tags:
  - javascript
  - testing
---

## Introduction

I mostly use Mocha testing framework with Chaijs assertion library.
Lately, I've seen a lot of open source projects using Jest testing platform.
I never thought about it since I've never had a problem with my current testing stack - Not untill I was trying to test time sensitive logic.

To solve this, I would use [Sinon](http://sinonjs.org/) where I would use [fake timers](http://sinonjs.org/releases/v2.2.0/fake-timers/). However, I'd have to a third dependancy to my unit testing stack in addition to Mocha and chaijs. That means more configuration overhead.

[Jest](http://facebook.github.io/jest/) claims to be a *zero configuration* testing platform. It comes with its own assertion and mock libraries. I immediately fell in love with how easy and fast I get going with testing.

Jest provides its own fake timers and below, I describe how to use them to test time sensitive logic.

## Problem

Let's say you have the following function that calls callback function x times every 1 second. How would we test it?

```javascript
const everySecond = (to=5, callback, i=0) => {
  setTimeout( () => {
    if (i++ === to) {
      return;
    }
    callback();
    everySecond(to, callback, i);
  }, 1000);
}
```

## Discussion

Let's first define our tests below:

* Should invoke callback once after 1 second.
* Should invoke callback three times after 3 seconds.
* Should invoke callback a maximum of 5 times give `to=5` as time goes to infinity.

## Solution

*Don't forget to checkout [the docs](https://facebook.github.io/jest/docs/en/timer-mocks.html#content) on timer mocks.*

 here is how we'd write our tests:

```javascript
jest.useFakeTimers();

test('Call callback 5 times every 1 second', () => {

  const callback = jest.fn();
  everySecond(5, callback);
  expect(callback).not.toBeCalled();

  jest.runTimersToTime(1000);                   // after 1 second
  expect(callback.mock.calls.length).toBe(1);
  jest.runTimersToTime(2999);                   // after 3 seconds
  expect(callback.mock.calls.length).toBe(3);
  jest.runTimersToTime(10000);                  // after infinity
  expect(callback.mock.calls.length).toBe(5);
});
```