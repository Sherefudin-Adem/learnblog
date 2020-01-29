+++
title = "Javascript ES6"
date = "2020-01-28"
draft = false
pinned = false
tags = ["Javascript", "ES6"]
image = "img/es6-Core.png"
description = "I will cover some new features in ES6. It will be helpful for someone who is new to ES6 or learning front-end frameworks."
footnotes = ""
+++
Topics I’m gonna cover are:

> + Let and Const             
> + Arrow functions
> + Default parameters        
> + for of loop
> + Spread attributes         
> + Maps
> + Sets                      
> + Static methods
> + Getters and Setters

# Let

`let` is similar to `var` but let has scope. let is only accessible in the `block level` it is defined.

```js
if (true) {
 let age = 30;
 console.log(age); //30
}
console.log(age); // undefined
```

In the above example variable `age` is defined inside If statement and so it's not accessible outside the function. Another example:

```js
let a = 50;
let b = 100;
if (true) {
 let a = 60;
 var c = 10;
 console.log(a/c); // 6
 console.log(b/c); // 10
}
console.log(c); // 10
console.log(a); // 50
```

# Const

`Const` is used to assign a constant value to the variable. And the value cannot be changed. Its fixed.

```js
const a = 50;
a = 60; // shows error. You cannot change the value of const.
const b = "Constant variable";
b = "Assigning new value"; // shows an error.
Consider another example.
const CITIES = ['Bern', 'Geneva', 'Rome', 'Paris'];
CITIES = "Tokyo"; // shows an error. 
CITIES.push('London'); // Works fine.
console.log(CITIES); // ['Bern', 'Geneva', 'Rome', 'Paris', 'london']
```

This may be little confusing. Consider in this way. Whenever you define a const variable, Javascript references the address of the value to the variable. In our example the variable `CITIES` actually references to the memory allocated to the array. So you cannot change the variable to reference some other memory location later. Throughout the program it only references to the array.

# Arrow Function

Functions in ES6 have changed a bit. I mean the syntax.

```js

// Old Syntax

function oldOne() {
 console.log("Hello Powercoders..!");
}

// New Syntax

var newOne = () => {
 console.log("Hello Powercoders..!");
}
```

The new syntax may be confusing a little bit. But I will try to explain the syntax. There are two parts of the syntax.

```js

var newOne = ()
=> {}

```

The first part is just declaring a variable and assigning the `function (i.e) ()` to it. It just says the variable is actually a function. Then the second part is declaring the body part of the function. The arrow part with the curly braces defines the body part. Another example with parameters.

```js

let withParameters = (a, b) => {
 console.log(a + b); // 30
}
withParameters(10, 20);

```

I don’t think I need to give explanation for this. Its straightforward.

# Default Parameters:

If someone is familiar with other programming Language like `Ruby`, `Python` then default parameters isn’t new to him/her. `Default parameters` are parameters which are given by default while declaring a function. But it’s value can be changed when calling the function. Example

```js

let Func = (a, b = 10) => {
 return a + b; 
}
Func(20); // 20 + 10 = 30

```

In the above example, i am passing only one parameter. The function makes use of the default parameter and executes the function. Consider another example:

```js

Func(20, 50); // 20 + 50 = 70

```

In the above example, the function takes two parameters and the second parameter replaces the default parameter. Consider another example:

```js

let NotWorkingFunction = (a = 10, b) => {
 return a + b;
}
NotWorkingFunction(20); // NAN. Not gonna work.

```

When you are calling the function with parameters they get assigned in the order. (i.e) the first value gets assigned to the first parameter and the second value gets assign to the second parameter and so on.. In the above example, the value 20 gets assigned to parameter `a` and `b` is not having any value. So we are not getting any output. But:-

```js

NotWorkingFunction(20, 30); // 50;

```

Works fine.

# For of loop

`for..of` is very similar to `for..in` with slight modification. for..of iterates through list of elements (i.e) like Array and returns the elements (not their index) one by one.

```js

let array = [2,3,4,1];
for (let value of array) {
 console.log(value);
}

```

### Output:

> 2 
> 3 
> 4 
> 1

`Note` that the variable `value` outputs each element in the array not the index. Another Example

```js

let string = "Sherefudin";
for (let char of string) {
 console.log(char);
}
```

### Output:

> S  
> h
> e 
> r
> e
> f
> u
> d
> i
> n

Yes. It works for string too.

# Spread attributes/operator

`Spread attributes` help to spread the expression as the name suggests. In simple words, it converts a list of elements to an array and vice versa. Example without spread attributes:

```js

let SumElements = (arr) => {
 console.log(arr); // [10, 20, 40, 60, 90]
 let sum = 0;
 for (let element of arr) {
  sum += element;
 }
 console.log(sum); // 220. 
}
SumElements([10, 20, 40, 60, 90]);

```

Above example is straightforward. We are declaring a function to accept array as parameter and returning its sum. It's simple. Now consider the same example with `spread attributes`

```js

let SumElements = (...arr) => {
 console.log(arr); // [10, 20, 40, 60, 90]
let sum = 0;
 for (let element of arr) {
  sum += element;
 }
 console.log(sum); // 220. 
}
SumElements(10, 20, 40, 60, 90);

```

`Note:` we are not passing array here. Instead we are passing the elements as arguments.  In the above example, the spread attribute converts the list of elements (i.e) the parameters to an array. Another Example:

```js

Math.max(10, 20, 60, 100, 50, 200); // returns 200.

```

`Math.max` is a simple method that returns the maximum element from given list. It doesn’t accept an array.

```js

let arr = [10, 20, 60];
Math.max(arr); // Shows error. Doesn't accept an array.
So let us use our savior.
let arr = [10, 20, 60];
Math.max(...arr); // 60
 
```

In the above example, the `spread attribute` converts the array to list of elements.

# Maps

`Map` holds key-value pairs. It’s similar to an array but we can define our own index. And indexes are unique in maps. Example:

```js

var NewMap = new Map();
NewMap.set('name', 'Adem'); 
NewMap.set('id', 8364759);
NewMap.set('interest', ['javascript', 'Swift', 'python']);
NewMap.get('name'); // Adem
NewMap.get('id'); // 8364759
NewMap.get('interest'); // ['javascript', 'Swift', 'python']

```

The Interesting features of Maps are all indexes are unique. And we can use any value as key or value. Example:

```js

var map = new Map();
map.set('name', 'Adem');
map.set('name', 'James');
map.set(1, 'number one');
map.set(NaN, 'No value');
map.get('name'); // James. Note Adem is replaced by James.
map.get(1); // number one
map.get(NaN); // No value
Other useful methods used in Map:
var map = new Map();
map.set('name', 'Adem');
map.set('id', 20);
map.size; // 2. Returns the size of the map.
map.keys(); // outputs only the keys. 
map.values(); // outputs only the values.
for (let key of map.keys()) {
 console.log(key);
}

```

### Output:

> name id

In the above example, `map.keys()` returns the keys of the map but it returns it in Iterator object. It means that it can’t be displayed as it is. It should be displayed only by iterating. Another example:

```js

var map = new Map();
for (let element of map) {
 console.log(element);
}

```

### Output:

> ['name', 'Adem'] \['id', 20]

The above example is self explanatory. The `for..of loop` outputs the `key-value pair` in an array. We can optimize it a little bit.

```js

var map = new Map();
for (let [key, value] of map) {
 console.log(key + " - " + value);
}

```

### Output:

> name - Adem id - 20

# Sets

`Sets` are used to store the unique values of any type. Simple..! Example

```js

var sets = new Set();
sets.add('a');
sets.add('b');
sets.add('a'); // We are adding duplicate value.
for (let element of sets) {
 console.log(element);
}

```

### Output:

> a b

`Note` that no duplicate values are displayed. Unique values are displayed. And also `note` that `sets are iterable objects`. We have to iterate through the elements to display it. Other useful methods:

```js

var sets = New Set([1,5,6,8,9]);
sets.size; // returns 5. Size of the size.
sets.has(1); // returns true. 
sets.has(10); // returns false.

```

In the above example, size is self-explanatory. There is another method `has` which return a boolean value based on whether the given element is present in the set or not.

# Static methods

Static methods are introduced in ES6. And it is pretty much easy to define it and use it.

### Example:

```js

class Example {
 static Callme() {
 console.log("I am static method");
 }
}
Example.Callme();

```

## Output:

> I am static method

`Note` that I didn’t use the keyword `function` inside `Class`. And I can call the function without creating any instance for the class.

## Getters and Setters

Getters and setters and one of the useful feature introduced in ES6. It will come in handy if you are using classes in JS. Example without getters and setters:

```js

class People {
constructor(name) {
      this.name = name;
    }
    getName() {
      return this.name;
    }
    setName(name) {
      this.name = name;
    }
}
let person = new People("Adem Hussein");
console.log(person.getName());
person.setName("James");
console.log(person.getName());

```

## Output:

> Adem 
> Hussein 
> James

I think the above example is self-explanatory. We have two functions in `class People` that helps to set and get the name of the person. Example with `getters` and `setters`

```js

class Person {
    constructor(name) {
      this.name = name;
    }
    get Name() {
      return this.name;
    }
    set Name(name) {
      this.name = name;
    }
}
let man = new Person("Sherefudin Adem");
console.log(man.Name);
man.Name = "Hussein";
console.log(man.Name);

```

In the above example, you can see there are two functions inside class People with `get` and `set` properties. The `get` property is used to get the value of the variable and `set` property is used to set the value to the variable.
