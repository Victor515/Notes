# Review for ES6 2

A glimpse at some ES6 features used quite frequently

## Spread Syntax  
Spread syntax, as the name suggests, is used to spread an array, a string, or an object wherever zero or more arguments(function calls), elements(array literals), or key-value pairs(object literals) are needed.  
Note that object spread syntax is not ES6, but will be standardized in **ECMAScript 2018**.
```javascript
function add(x, y) {
  return x + y;
}
// function calls
const numbers = [4, 38];
add(...numbers) // 42

// array literals
console.log(1, ...[2, 3, 4], 5)
// 1 2 3 4 5

// more than one spread
[1, 2, ...[3, 4, 5], ...[6, 7]];

// object literals
let objClone = { ...obj };
```

### Replace apply()  
In ES5, apply() is used convert array into arguments. With spread syntax, there is no need for that.  
```javascript
// ES5
function f(x, y, z) {
  // ...
}
var args = [0, 1, 2];
f.apply(null, args);

// ES6
function f(x, y, z) {
  // ...
}
let args = [0, 1, 2];
f(...args);
```

### Copy arrays or objects  
1. Array
```javascript
var arr = [1, 2, 3];
var arr2 = [...arr]; // like arr.slice()
arr2.push(4); 

// arr2 becomes [1, 2, 3, 4]
// arr remains unaffected
```
Note that spread syntax effectively goes one level deep while copying an array, so it may not be suitable to copy multidimensional arrays.
```javascript
var a = [[1], [2], [3]];
var b = [...a];
b.shift().shift(); // 1
// Now array a is affected as well: [[], [2], [3]]
```
2. Object(ES7)  
Spread syntax copies own enumerable properties from a provided object onto a **new** object. Note that it is **shallow copy(excluding prototype)**.  
Shallow-cloning or merging of objects is now possible using a shorter syntax than `Object.assign()`.
```javascript
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };

var clonedObj = { ...obj1 };
// Object { foo: "bar", x: 42 }

var mergedObj = { ...obj1, ...obj2 };
// Object { foo: "baz", x: 42, y: 13 }
```


### Integration with [destructing assignment](#Destructing-Assignment)  
```javascript
// ES5
a = list[0], rest = list.slice(1)
// ES6
[a, ...rest] = list

const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```

### String Literals  
```javascript
[...'hello']
// [ "h", "e", "l", "l", "o" ]
```

### Other iterables(any object implementing Iterator Interface)  
Here `querySelectorAll` is a `nodeList` object, which implements the `Iterator` Interface.
```javascript
let nodeList = document.querySelectorAll('div');
let array = [...nodeList]
```

## Rest Parameter  
Rest parameter is used when we have indefinite number of arguments passed to an array. It is generally considered a replacement for [arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments) object in previous version.  
``` javascript
function f(a, b, ...theArgs) {
  // ...
  console.log(theArgs);
}

f(1, 2, 3, 4, 5) // [3, 4, 5]
```

A few points to note:  
1. Unlike arguments, rest parameters are stored in a **real** array.
2. There should be no more arguments after rest parameters.
```javascript
// Error!
function f(a, ...b, c) {
  // ...
}
```  
3. The array length does not take rest into account.
```javascript
(function(a) {}).length  // 1
(function(...a) {}).length  // 0
(function(a, ...b) {}).length  // 1
```  
4. Rest can be used together with [destructing assignment](#Destructing-Assignment)  
```javascript
function f(...[a, b, c]) {
  return a + b + c;
}

f(1)          // NaN (b and c are undefined)
f(1, 2, 3)    // 6
f(1, 2, 3, 4) // 6 (the fourth parameter is not destructured)
```

## Destructing Assignment  
The destructuring assignment syntax is used to **unpack** values from arrays, or properties from objects, into distinct variables.  
Note: It is very common to use destructing assignment with [rest parameter](#Rest-Parameter) together.

### Array Destructing
```javascript
let a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

// destruct array with rest parameters
[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]
```

### Object Destructing  
In array destructing, we use indexing to do match left-hand and right hand. In object destructing, however, there is such ordered indexing. As a result, we need to provide key in left hand side to match the right hand side.  
``` javascript
({ a, b } = { a: 10, b: 20 });
console.log(a); // 10
console.log(b); // 20


// Stage 3 proposal
({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}

// Note that the round braces is needed for object destructing without declaration
let a, b;

({a, b} = {a: 1, b: 2});
```

What if we want assign the value to a variable with new name?  
``` javascript
let o = {p: 42, q: true};
let {p: foo, q: bar} = o;
 
console.log(foo); // 42 
console.log(bar); // true
```

### Default values  
A variable can be assigned a default, in the case that the value unpacked from the array/object is undefined.  
``` javascript
// array
let a, b;

[a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7

// object
let {a = 10, b = 5} = {a: 3};

console.log(a); // 3
console.log(b); // 5
```


## Module

## Template Literals

## Computed property names

## Default parameters
