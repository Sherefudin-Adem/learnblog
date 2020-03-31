+++
title = "Class Composition and constructor Functions"
date = "2020-03-30"
draft = false
pinned = false
tags = ["Class Composition and constructor Functions"]
image = "../img/constructor.png"
description = "I will explain about Class Composition and constructor Functions in JavaScript "
footnotes = ""
+++


# Class Composition and constructor Functions in JavaScript 

composition gives us more flexibility by composing functionality to create a new object, while inheritance forces us to extend entities in an inheritance hierarchy

## Inheritance with Classes

ES26 introduced the class syntax to JavaScript. The syntax gives us a nice way to use Object Oriented Programming (OOP) compared to managing prototypes. For example, inheritance in ES5 with prototypes:

```js

const Animal = function(name) {
  this.name = name;
}

const Crocodile = function(name) {
  Animal.apply(this, arguments); // Call parent constructor
}

// Extend the prototype
Crocodile.prototype = Object.create(Animal.prototype);
Crocodile.prototype.constructor = Crocodile;

const bob = new Crocodile("bob");

//Becomes this using ES 2015 classes:

class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Crocodile extends Animal {}

const bob = new Crocodile("bob");
//With ES26 classes, you can omit the constructor, then the parent one will be called. If you wish to make it explicit, it’d be equivalent to:

class Crocodile extends Animal {
  constructor(...args) {
    super(...args);
  }
}

```
The new class syntax is just syntactical sugar over the prototype-based model and behind the scenes prototypes are still being used.

The bottom line is that classes are functions, and functions are objects in JavaScript, extendable using its prototypal inheritance nature. That sounds confusing, but it gives the language a lot of flexibility.

## Object Composition
A common composition pattern in JavaScript is using object composition. It combines the power of objects and functional programming. For the example above, imagine an animal has the ability to eat and fly. In hierarchy, that could mean to have an Animal and FlyingAnimal. And if we add more and more animals, that hierarchy could become a bit messy, since abilities are shared between animals.

With composition, you could have factories that create an object:

```js

const Crocodile = name => {
  const self = {
    name
  };

  return self;
}

const bob =  Crocodile("bob");

```

We’re using an internal variable *self* that would represent the prototype using classes or prototypes. This would behave exactly as the example above.

Then, you can define behaviors as functions receiving the self. That makes them easily composable since they’re just functions. Then we’ll use any object merging utility, such as Object.assign or the spread operator *({...a, ...b})* in the factory function in order to create the final object:

```js

// We have some behaviors
const canSayHi = self => ({
  sayHi: () => console.log(`Hi! I'm ${self.name}`)
});
const canEat = () => ({
  eat: food => console.log(`Eating ${food}...`)
});
const canPoop = () => ({
  poop: () => console.log('Going to poooo...')
});

// Combined previous behaviours
const socialBehaviors = self => Object.assign({}, canSayHi(self), canEat(), canPoop());

const Crocodile = name => {
  const self = {
    name
  };

  const CrocodileBehaviors = self => ({
    bite: () => console.log("Yum yum!")
  });

  return Object.assign(self, socialBehaviors(self), CrocodileBehaviors(self));
};


const bob = Crocodile("Bob");
bob.sayHi(); // Hi! I'm Bob
bob.eat("Banana"); // Eating Banana...
bob.bite(); // Yum yum!

```
As you can see, we define different behaviors prefixed with can (the with prefix is usually used as well). We are even combining some of them into socialBehaviors by creating a new composed object.

In this way, it’d be quite easy to create another animal:

```js

const dog = name => {
  const self = {
    name
  };

  const dogBehaviors = self => ({
    bark: () => console.log("Woff woff!"),
    haveLunch: food => {
      self.eat(food);
      self.poop();
    }
  });

  return Object.assign(self, dogBehaviors(self), canEat(), canPoop());
}

```

Keep in mind that we’re appending all functionality into the same reference of self, that’s why you can call self.eat within haveLunch. That allow us to create behaviors on top of other behaviors.

This kind of composition has the benefits of easy refactoring and a simple mental model for structuring since you don’t have the restrictions of a hierarchy.

### Composition with JavaScript Classes
All of this is so cool, but lots of people are used to the OOP way and prefer to work with ES26 classes. But remember that classes are functions and functions are objects, so we can compose them as well.

We can use a mixin technique in order to define pieces of behaviors, consisting of a factory function that takes a superclass as a parameter and returns a subclass:

```js

// Create a mixin
const FoodMixin = superclass => class extends superclass {
  eat(food) {
    console.log(`Eating ${food}`);
  }

  poop() {
    console.log("Going to poooo...");
  }
};

```
Then we can use it to reproduce the Dog example by enhancing an Animal class with the FoodMixin:

```js

class Animal {
  constructor(name) {
    this.name = name
  }
}

class Dog extends FoodMixin(Animal) {
  constructor(...args) {
    super(...args)
  }

  bark() {
    console.log("Woff woff!")
  }

  haveLunch(food) {
    this.eat(food);
    this.poop();
  }
}

const bob = new Dog("Bob");
bob.haveLunch("bones");

```
Using classes for composing gives us both the advantages of the class inheritance and composition world: you can compose behaviors by using a familiar OOP syntax and structure, where *super* and *this* are preserved because of JavaScript’s prototype chain.

### Combining Mixins
Since mixins are just factory functions, we can use several of them:

```js

const MixinA = superclass => class extends superclass {};
const MixinB = superclass => class extends superclass {};

class Base {}
class Child extends MixinB(MixinA(Base)) {}
We can also create mixins that extend other mixins, although that creates dependencies so try to not overuse it:

const MixinA = superclass => class extends superclass {};
const MixinB = superclass => class extends MixinA(superclass) {};

class Base {}
class Child extends MixinB(Base) {}

```
The problem using several mixins is that we easily end up in a deep nested syntax:

```js

const MixinA = superclass =>class extends superclass {};
const MixinB = superclass => class extends superclass {};
const MixinC = superclass => class extends superclass {};
const MixinD = superclass => class extends superclass {};

class Base {}
class Child extends MixinD(MixinC(MixinB(MixinA(Base)))) {}

```
The cool thing is that since they’re just unary pure functions (they take only one argument), we can use a compose functional utility to avoid that, such as lodash’s one:

```js

import compose from "lodash/fp/compose"

const MixinA = superclass => class extends superclass {};
const MixinB = superclass => class extends superclass {};
const MixinC = superclass => class extends superclass {};

class Base {}

const Behaviors = compose(MixinA, MixinB, MixinC)(Base)

class Child extends Behaviors {}
```

As a final example, let’s create a more “real-world” super powered Dog example. We could move all behaviors to a behaviors file:

#### behaviors

```js

export const EatMixin = superclass => class extends superclass {
  eat(food) {
    console.log(`Eating ${food}`);
  }
};

export const PoopMixin = superclass => class extends superclass {
  poop() {
    console.log("Going to poooo...");
  }
};

export const FlyMixin = superclass => class extends superclass {
  fly() {
    console.log("Flying for real!");
  }
};

```

And in a dog-example.js file use them:

```js


class Animal {
  constructor(name) {
    this.name = name
  }
}

const SuperPoweredDog = compose(EatMixin, PoopMixin, FlyMixin)(Animal);

class Dog extends SuperPoweredDog {
  bark() {
    console.log("Woff woff!")
  }

  haveLunch(food) {
    this.eat(food);
    this.poop();
  }
}

const bob = new Dog("Bob");
bob.bark(); // Woff woff!
bob.haveLunch("bones"); // Eating bones. Going to poooo...

```

> Note that we don't need to specify a constructor in the Dog class if we're just calling a parent constructor with all arguments. That happens implicitly.

