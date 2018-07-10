# Review for ES6 2

A glimpse at some ES6 features used quite frequently

## Spread Syntax

## Rest Parameter

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
