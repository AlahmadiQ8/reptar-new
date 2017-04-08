---
title: General Interview Questions
date: 2017-4-7
tags:
- meta
---
# General Interview Questions


## Table of Contents


* [General Javascript](#general-javascript)
  * [Write a function that sums a variable number of arguments\. Then apply the function to sum an array](#write-a-function-that-sums-a-variable-number-of-arguments-then-apply-the-function-to-sum-an-array)
  * [Implement class inheritance in vanilla javascript](#implement-class-inheritance-in-vanilla-javascript)
  * [Can you name two programming paradigms important for JavaScript app developers?](#can-you-name-two-programming-paradigms-important-for-javascript-app-developers)
  * [What is functional programming?](#what-is-functional-programming)
  * [What are two\-way data binding and one\-way data flow, and how are they different?](#what-are-two-way-data-binding-and-one-way-data-flow-and-how-are-they-different)
  * [What is asynchronous programming, and why is it important in JavaScript?](#what-is-asynchronous-programming-and-why-is-it-important-in-javascript)
  * [What is prototypal inheritance?](#what-is-prototypal-inheritance)
  * [An Object](#an-object)
  * [Prototype Chain](#prototype-chain)
  * [Constructor](#constructor)
* [Javascript Puzzle Questions](#javascript-puzzle-questions)
  * [Write a function that can determine whether a string is a palindrome in under 100 characters](#write-a-function-that-can-determine-whether-a-string-is-a-palindrome-in-under-100-characters)
  * [How would you empty the array var arr = [1,2,3,4]?](#how-would-you-empty-the-array-var-arr--1234)
  * [Write a mul function which mul(2)(3)(4) = 24](#write-a-mul-function-which-mul234--24)



## General Javascript 

### Write a function that sums a variable number of arguments. Then apply the function to sum an array

```javascript
function sum() {
  let res = 0;
  for (let i = 0; i< arguments.length; i++) {
    res += arguments[i];
  }
  return result;
}
sum(1,2,3); // 6
var data = [1,2,3];
sum.apply(null, data); // 6
```

[Source](http://stackoverflow.com/a/2494047/5431968)


### Implement class inheritance in vanilla javascript

```javascript

function Person(first, last) {
  this.name = name;
  this.last = last;
};

Person.prototype.greeting = function() {
  alert('Hi! I\'m ' + this.name.first + '.');
};

var person1 = new Person('Tammi', 'Smith');

function Teacher(first, last, subject) {
  Person.call(this, first, last);
  this.subject = subject;
}

Teacher.prototype = Object.create(Person.prototype);
Teacher.prototype.constructor = Teacher;

var teacher1 = new Teacher('Dave', 'Griffiths', 'mathematics');
```

[Source](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Inheritance)

### Can you name two programming paradigms important for JavaScript app developers?

* Prototypal inheritance (also: prototypes, OLOO).
* Functional programming (also: closures, first class functions, lambdas).

[Source](https://medium.com/javascript-scene/10-interview-questions-every-javascript-developer-should-know-6fa6bdf5ad95)

### What is functional programming?

* Pure functions / function puri
* No shared states & mutable data
* Avoid side-effects
* Simple function composition

[Source](https://medium.com/javascript-scene/10-interview-questions-every-javascript-developer-should-know-6fa6bdf5ad95)

### What are two-way data binding and one-way data flow, and how are they different?

Two way data binding means that UI fields are bound to model data dynamically 
such that when a UI field changes, the model data changes with it and vice-versa.

One way data flow means that the model is the single source of truth. Changes 
in the UI trigger messages that signal user intent to the model (or “store” in React). 
Only the model has the access to change the app’s state. The effect is that data 
always flows in a single direction, which makes it easier to understand.

[Source](https://medium.com/javascript-scene/10-interview-questions-every-javascript-developer-should-know-6fa6bdf5ad95)

### What is asynchronous programming, and why is it important in JavaScript?

Synchronous programming means that, barring conditionals and function calls, 
code is executed sequentially from top-to-bottom, blocking on long-running tasks 
such as network requests and disk I/O.

Asynchronous programming means that the engine runs in an event loop. When a 
blocking operation is needed, the request is started, and the code keeps running 
without blocking for the result. When the response is ready, an interrupt is fired, 
which causes an event handler to be run, where the control flow continues. 
In this way, a single program thread can handle many concurrent operations.

[Source](https://medium.com/javascript-scene/10-interview-questions-every-javascript-developer-should-know-6fa6bdf5ad95)

### What is prototypal inheritance?

In a classical language, classes typically define the structure of objects, but 
in a prototypal language, the objects themselves define their structure, and 
this structure can be inherited and modified by other objects at runtime.

Prototypal inheritance first appeared in Self and has since appeared in many 
other languages, but these days most people think of JavaScript when they think 
of prototypal inheritance.

### An Object


```javascript
var foo = {
  x: 10,
  y: 20
};
```

![](http://dmitrysoshnikov.com/wp-content/uploads/basic-object.png)

[source](http://dmitrysoshnikov.com/ecmascript/javascript-the-core/)

### Prototype Chain


```javascript
var a = {
  x: 10,
  calculate: function (z) {
    return this.x + this.y + z;
  }
};
 
var b = {
  y: 20,
  __proto__: a
};
 
var c = {
  y: 30,
  __proto__: a
};

// The above is equivelant to 
// var b = Object.create(a, {y: {value: 20}});
// var c = Object.create(a, {y: {value: 30}});
 
// call the inherited method
b.calculate(30); // 60
c.calculate(40); // 80
```

![](http://dmitrysoshnikov.com/wp-content/uploads/prototype-chain.png)

[source](http://dmitrysoshnikov.com/ecmascript/javascript-the-core/)


### Constructor

Besides creation of objects by specified pattern, a constructor function does 
another useful thing — it automatically sets a prototype object for newly created 
objects. This prototype object is stored in the ConstructorFunction.prototype 
property.

E.g., we may rewrite previous example with b and c objects using a constructor 
function. Thus, the role of the object a (a prototype) Foo.prototype plays:

```javascript
// a constructor function
function Foo(y) {
  // which may create objects
  // by specified pattern: they have after
  // creation own "y" property
  this.y = y;
}
 
// also "Foo.prototype" stores reference
// to the prototype of newly created objects,
// so we may use it to define shared/inherited
// properties or methods, so the same as in
// previous example we have:
 
// inherited property "x"
Foo.prototype.x = 10;
 
// and inherited method "calculate"
Foo.prototype.calculate = function (z) {
  return this.x + this.y + z;
};
 
// now create our "b" and "c"
// objects using "pattern" Foo
var b = new Foo(20);
var c = new Foo(30);
 
// call the inherited method
b.calculate(30); // 60
c.calculate(40); // 80
 
// let's show that we reference
// properties we expect
 
console.log(
 
  b.__proto__ === Foo.prototype, // true
  c.__proto__ === Foo.prototype, // true
 
  // also "Foo.prototype" automatically creates
  // a special property "constructor", which is a
  // reference to the constructor function itself;
  // instances "b" and "c" may found it via
  // delegation and use to check their constructor
 
  b.constructor === Foo, // true
  c.constructor === Foo, // true
  Foo.prototype.constructor === Foo, // true
 
  b.calculate === b.__proto__.calculate, // true
  b.__proto__.calculate === Foo.prototype.calculate // true
 
);
```

![](http://dmitrysoshnikov.com/wp-content/uploads/constructor-proto-chain.png)

[Source](http://dmitrysoshnikov.com/ecmascript/javascript-the-core/)


## Javascript Puzzle Questions

### Write a function that can determine whether a string is a palindrome in under 100 characters

```javascript
function isPalindrome(str) {
  return (str.toLowerCase() == str.split('').reverse().join(''));
}
```

[Source](https://www.upwork.com/i/interview-questions/javascript/)

### How would you empty the array ```var arr = [1,2,3,4]```?

```javascript
arr.length = 0;

arr.splice(0, arr.length);

while(arr.length){
    arr.pop();
}

arr = []
```

[Source](https://www.upwork.com/i/interview-questions/javascript/)


### Write a ```mul``` function which ```mul(2)(3)(4) = 24```

```javascript
function mul (x) {
  return function (y) { // anonymous function 
    return function (z) { // anonymous function 
      return x * y * z; 
    };
  };
}
```


### Prototypal Inheritance - What will be the output of the code below? 

```javascript
var Employee = {
  company: 'xyz'
}
var emp1 = Object.create(Employee);
delete emp1.company
console.log(emp1.company);
```

The output would be ```xyz```. Here, ```emp1``` object has ```company``` as its prototype property. 
The ```delete``` operator doesn't delete prototype property.

emp1 object doesn't have company as its own property. You can test it 
```console.log(emp1.hasOwnProperty('company')); //output : false```. 
However, we can delete the company property directly from the ```Employee``` object 
using ```delete Employee.company```. Or, we can also delete the ```emp1``` object using the 
```__proto__``` property ```delete emp1.__proto__.company```.

[Source](https://www.codementor.io/nihantanu/21-essential-javascript-tech-interview-practice-questions-answers-du107p62z)



<!-- ### Basic JS programmming

* Scope of variable
* What is Associative Array? How do we use it?

### OOPS JS

* Difference between Classic Inheritance and Prototypical Inheritance
* What is difference between private variable, public variable and static variable? How we achieve this in JS?
* How to add/remove properties to object in run time?
* How to achieve inheritance ?
* How to extend built-in objects?
* Why extending array is bad idea?

### DOM and JS

* Difference between browser detection and feature detection
* DOM Event Propagation
* Event Delegation
* Event bubbling V/s Event Capturing

###Misc

Graceful Degradation V/s Progressive Enhancement


 -->