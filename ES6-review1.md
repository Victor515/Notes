# Review for ES6

Just a glimpse at some common ES6 features.

## Arrow Function
An arrow function expression has a shorter syntax than a function expression and does not have its **own** this, [arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments), super, or [new.target](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new.target).
These function expressions are best suited for **non-method functions**, and they cannot be used as constructors.

### Basic syntax(be aware of the last case):
```javascript
(param1, param2, …, paramN) => { statements }
(param1, param2, …, paramN) => expression
// equivalent to: => { return expression; }

// Parentheses are optional when there's only one parameter name:
(singleParam) => { statements }
singleParam => { statements }

// The parameter list for a function with no parameters should be written with a pair of parentheses.
() => { statements }
```

### Comment:
Two factors influenced the introduction of arrow functions: *shorter functions* and *non-binding of this*.

See this example:
```javascript
function Person() {
  // The Person() constructor defines `this` as an instance of itself.
  this.age = 0;

  setInterval(function growUp() {
    // In non-strict mode, the growUp() function defines `this`
    // as the global object (because it's where growUp() is executed.),
    // which is different from the `this`
    // defined by the Person() constructor.
    this.age++;
  }, 1000);
}

var p = new Person();
```

Traditional (ES3, ES5) solutions:
```javascript
function Person() {
  var that = this;
  that.age = 0;

  setInterval(function growUp() {
    // The callback refers to the `that` variable of which
    // the value is the expected object.
    that.age++;
  }, 1000);
}
```
Or we could use `bind()`[function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

**ES6 solution**:
An arrow function does not have its own this; the this value of **the enclosing execution context** is used. Thus, in the following code, the this within the function that is passed to setInterval has the same value as this in the enclosing function:
```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the Person object
  }, 1000);
}

var p = new Person();
```

### Addition:
1. Arrow functions do not have a prototype property.
2. The yield keyword may not be used in an arrow function's body (except when permitted within functions further nested within it). As a consequence, arrow functions cannot be used as generators.

## Class
JavaScript classes, introduced in ECMAScript 2015, are primarily **syntactical sugar** over [JavaScript's existing prototype-based inheritance](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain). The class syntax **does not** introduce a new object-oriented inheritance model to JavaScript.

### Define a class
1. Class Declaration:
```javascript
class Rectangle {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
}
```
2. Class expressions  
```javascript
// unnamed
let Rectangle = class {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// output: "Rectangle"

// named
let Rectangle = class Rectangle2 {
  constructor(height, width) {
    this.height = height;
    this.width = width;
  }
};
console.log(Rectangle.name);
// output: "Rectangle2"
```

### Strict mode in Class
Code within the class syntax is always executed in **strict mode**, see this example:
```javascript
class Animal {
  speak() {
    return this;
  }
  static eat() {
    return this;
  }
}

let obj = new Animal();
obj.speak(); // Animal {}
let speak = obj.speak;
speak(); // undefined

Animal.eat() // class Animal
let eat = Animal.eat;
eat(); // undefined
```

What will happen in traditional mode?
```javascript
function Animal() { }

Animal.prototype.speak = function() {
  return this;
}

Animal.eat = function() {
  return this;
}

let obj = new Animal();
let speak = obj.speak;
speak(); // global object

let eat = Animal.eat;
eat(); // global object
```
**Explanation**:  
Autoboxing in method calls will happen in non–strict mode based on the initial this value. If the inital value is undefined, this will be set to the global object.

Autoboxing will not happen in strict mode, the this value remains as passed.

### Sub classing with extends  
