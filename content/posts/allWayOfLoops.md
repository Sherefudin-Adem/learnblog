+++
title = "JavaScript Loops"
date = "2020-04-06"
draft = false
pinned = false
tags = ["All possible way of iterating through Loops"]
image = "../img/JavaScriptLoops.jpg"
description = "I will try to explain about loops in JavaScript "
footnotes = ""
+++


## What is a JavaScript Loop?
JavaScript Loop provides a quick and easy way to do repetitive tasks. They offer to perform iterations with only a few lines of code. Iteration is the number of times you want to repeat the task (that number can even be zero). Be sure you understand all of them, so you can implement the best loop statement for a given situation.

__CONTENTS__
+ for
+ forEach
+ do...while
+ while
+ for...in
+ for...of

### Introduction
JavaScript provides many way to iterate through loops. In this post i will try to explain each one with a small example and the main properties.

### for

JavaScript *for* loops consists of three parts, separated with a semicolon:

__Initialization__: It initializes the loop with an iteration variable and executes once at the beginning of the loop.
__Condition__: It specifies a certain condition in the loop that determines whether the loop body will execute or not. If the condition returns true, the code inside the for loop will execute. If it returns false, the control moves onto the statement after the loop.
__Increment/ Decrement__: This section increments or decrements the value of our iteration variable by 1.

These three sections are not compulsory, but make sure you remember the semicolons. The syntax of for statement looks like this:


```js
const animals = ['aardvark', 'buffalo', 'cow', 'lion', 'cheeta', 'tiger']
for (let i = 0; i < animals.length; i++) {
  console.log(animals[i]) //value
  console.log(i) //index
}
```
You can interrupt a for loop using break
You can fast forward to the next iteration of a for loop using continue

> This loop is useful when you know how many times you want the loop to execute. This loop also reduces the total lines in your code to achieve the same task.

### forEach
Given an array, you can iterate over its properties using *animals.forEach()*:

```js
const animals = ['aardvark', 'buffalo', 'cow', 'lion', 'cheeta', 'tiger']
animals.forEach((item, index) => {
  console.log(item) //value
  console.log(index) //index
})

//index is optional
animals.forEach(item => console.log(item))

```
unfortunately you cannot break out of this loop.

### do...while
This is an exit controlled loop; the loop body executes at least once, even if the condition is not true. This is because the condition is tested when the loop body ends. If the condition returns true, the code inside the loop executes again. If it returns false, the JavaScript interpreter exits the loop.

The syntax of a *do…while* statement is as follows:

```js
const animals = ['aardvark', 'buffalo', 'cow', 'lion', 'cheeta', 'tiger']
let i = 0
do {
  console.log(animals[i]) //value
  console.log(i) //index
  i++
} while (i < animals.length)

```
You can interrupt a while loop using break:

```js

do {
  if (something) break
} while (true)

```

and you can jump to the next iteration using continue:

```js
do {
  if (something) continue

  //do something else
} while (true)

```

### while

A while statement in JavaScript executes until our boolean condition evaluates to true. This loop is an entry controlled loop.

- If the test condition returns false, then control passes to the statement just after the loop.
- If the test condition returns true, the loop body is executed and the condition is tested again.
The syntax for a while statement is as follows:

```js

const animals = ['aardvark', 'buffalo', 'cow', 'lion', 'cheeta', 'tiger']
let i = 0
while (i < animals.length) {
  console.log(animals[i]) //value
  console.log(i) //index
  i++
}

```
You can interrupt a while loop using break:
```js

while (true) {
  if (something) break
}

```

and you can jump to the next iteration using continue:

```js

while (true) {
  if (something) continue

  //do something else
}

```

The difference with do...while is that do...while always execute its cycle at least once.

### for...in
It doesn’t require the programmer to know the number of iterations the loop needs to perform. It creates a loop iterating over all the enumerable properties of an object. The keys store the key of the current property and we access the property’s value with it. This means that the loop executes a specific set of statements for each distinct property in a JavaScript Object. 

The syntax of a for…in statement in JavaScript looks like this:
Iterates all the enumerable properties of an object, giving the property names.

```js
const numbers = [1, 4, 9, 8, 5, 3, 6];
for (let property in numbers) {
  console.log(property) //property name
  console.log(numbers[property]) //property value
}

```
The reason why the developers created the for…in loop is to work with user-defined properties in an object. It is better to use a traditional for loop over Array elements with numeric indexes.

### for...of

In the last JavaScript statement, we had to access the array’s values with the help of square brackets [ ]. We couldn’t access them directly and it makes your program more error-prone. So the JavaScript developers got us a new way to access these values directly. This makes our code a lot more efficient than before because the values store the property’s value, not the key.

ES6 introduced the for...of loop, which combines the conciseness of forEach with the ability to break:

```js
//iterate over the value
const animals = ['aardvark', 'buffalo', 'cow', 'lion', 'cheeta', 'tiger']
for (const value of animals) {
  console.log(value) //value
}

//get the index as well, using `entries()`
const animals = ['aardvark', 'buffalo', 'cow', 'lion', 'cheeta', 'tiger']
for (const [index, value] of animals.entries()) {
  console.log(index) //index
  console.log(value) //value
}

```

> *Notice* the use of *const* inside for...of & for...in. This loop creates a new scope in every iteration, so we can safely use that instead of let.


__for...of__ iterates over the property values
__for...in__ iterates the property names
