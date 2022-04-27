## Refresher:

A function is a named and parameterized block of JavaScript code you define once, and can then invoke over and over again. 
    
   - Here are some simple examples:

```
// Functions are parameterized blocks of JavaScript code that we can invoke.
function plus1(x) {        // Define a function named "plus1" with parameter "x"
    return x + 1;          // Return a value one larger than the value passed in
}                          // Functions are enclosed in curly braces

plus1(y)                   // => 4: y is 3, so this invocation returns 3+1

let square = function(x) { // Functions are values and can be assigned to vars
    return x * x;          // Compute the function's value
};                         // Semicolon marks the end of the assignment.

square(plus1(y))           // => 16: invoke two functions in one expression
```


### Arrow function

In ES6 and later, there is a shorthand syntax for defining functions. This concise syntax uses ```=>``` to separate the argument list from the function body, so functions defined this way are known as **arrow functions**. Arrow functions are most commonly used when you want to pass an unnamed function as an argument to another function. The preceding code looks like this when rewritten to use arrow functions:

```
const plus1 = x => x + 1;   // The input x maps to the output x + 1
const square = x => x * x;  // The input x maps to the output x * x
plus1(y)                    // => 4: function invocation is the same
square(plus1(y))            // => 16
```

### Methods

When we use functions with objects, we get methods:

```
// When functions are assigned to the properties of an object, we call
// them "methods."  All JavaScript objects (including arrays) have methods:
let a = [];                // Create an empty array
a.push(1,2,3);             // The push() method adds elements to an array
a.reverse();               // Another method: reverse the order of elements

// We can define our own methods, too. The "this" keyword refers to the object
// on which the method is defined: in this case, the points array from earlier.
points.dist = function() { // Define a method to compute distance between points
    let p1 = this[0];      // First element of array we're invoked on
    let p2 = this[1];      // Second element of the "this" object
    let a = p2.x-p1.x;     // Difference in x coordinates
    let b = p2.y-p1.y;     // Difference in y coordinates
    return Math.sqrt(a*a + // The Pythagorean theorem
                     b*b); // Math.sqrt() computes the square root
};
points.dist()              // => Math.sqrt(2): distance between our 2 points
```


## Now, as promised, here are some functions whose bodies demonstrate common JavaScript control structure statements:

```
// JavaScript statements include conditionals and loops using the syntax
// of C, C++, Java, and other languages.

function abs(x) {          // A function to compute the absolute value.
    if (x >= 0) {          // The if statement...
        return x;          // executes this code if the comparison is true.
    }                      // This is the end of the if clause.
    else {                 // The optional else clause executes its code if
        return -x;         // the comparison is false.
    }                      // Curly braces optional when 1 statement per clause.
}                          // Note return statements nested inside if/else.
abs(-10) === abs(10)       // => true

function sum(array) {      // Compute the sum of the elements of an array
    let sum = 0;           // Start with an initial sum of 0.
    for(let x of array) {  // Loop over array, assigning each element to x.
        sum += x;          // Add the element value to the sum.
    }                      // This is the end of the loop.
    return sum;            // Return the sum.
}
sum(primes)                // => 28: sum of the first 5 primes 2+3+5+7+11

function factorial(n) {    // A function to compute factorials
    let product = 1;       // Start with a product of 1
    while(n > 1) {         // Repeat statements in {} while expr in () is true
        product *= n;      // Shortcut for product = product * n;
        n--;               // Shortcut for n = n - 1
    }                      // End of loop
    return product;        // Return the product
}
factorial(4)               // => 24: 1*4*3*2

function factorial2(n) {   // Another version using a different loop
    let i, product = 1;    // Start with 1
    for(i=2; i <= n; i++)  // Automatically increment i from 2 up to n
        product *= i;      // Do this each time. {} not needed for 1-line loops
    return product;        // Return the factorial
}
factorial2(5)              // => 120: 1*2*3*4*5
```

## Functions as Values

The most important features of functions are they can be defined and invoked. 
    - Function definition and invocation are syntactic features of JavaScript and of most other programming languages. 
In JavaScript, however, functions are not only syntax but also values, which means they can be; 
    - assigned to variables, 
    - stored in the properties of objects or the elements of arrays, 
    - passed as arguments to functions, and so on.

To understand how functions can be JavaScript data as well as JavaScript syntax, consider this function definition:

```
function square(x) { return x*x; }
```
This definition;
  - creates a new function object and 
  - assigns it to the variable square. 
  
The name of a function is really immaterial; it is simply the name of a variable which refers to the function object. The function can be assigned to another variable and still work the same way:

```
let s = square;  // Now s refers to the same function that square does
square(4)        // => 16
s(4)             // => 16
```

Functions can also be assigned to object properties rather than variables. 
  - We call the functions “methods” when we do this:

```
let o = {square: function(x) { return x*x; }}; // An object literal
let y = o.square(16);       
// y == 256
```

Functions don’t even require names at all, as when they’re assigned to array elements:

```
let a = [x => x*x, 20]; // An array literal
a[0](a[1])              // => 400
```

The syntax of this last example looks strange, but it is still a legal function invocation expression!

The example code below demonstrates the kinds of things which can be done when functions are used as values. This example may be a little tricky, but the comments explain what is going on.

```
// We define some simple functions here
function add(x,y) { return x + y; }
function subtract(x,y) { return x - y; }
function multiply(x,y) { return x * y; }
function divide(x,y) { return x / y; }

// Here's a function that takes one of the preceding functions
// as an argument and invokes it on two operands
function operate(operator, operand1, operand2) {
    return operator(operand1, operand2);
}

// We could invoke this function like this to compute the value (2+3) + (4*5):
let i = operate(add, operate(add, 2, 3), operate(multiply, 4, 5));

// For the sake of the example, we implement the simple functions again,
// this time within an object literal;
const operators = {
    add:      (x,y) => x+y,
    subtract: (x,y) => x-y,
    multiply: (x,y) => x*y,
    divide:   (x,y) => x/y,
    pow:      Math.pow  // This works for predefined functions too
};

// This function takes the name of an operator, looks up that operator
// in the object, and then invokes it on the supplied operands. Note
// the syntax used to invoke the operator function.
function operate2(operation, operand1, operand2) {
    if (typeof operators[operation] === "function") {
        return operators[operation](operand1, operand2);
    }
    else throw "unknown operator";
}

operate2("add", "hello", operate2("add", " ", "world")) // => "hello world"
operate2("pow", 10, 2)  // => 100
```

## Higher-Order Functions

A higher-order function is a; 
  - **function which operates on functions***, 
  - taking one or more functions as arguments and 
  - returning a new function. 
  
  Here is an example:

```
// This higher-order function returns a new function that passes its
// arguments to f and returns the logical negation of f's return value;

function not(f) {
    return function(...args) {             // Return a new function
        let result = f.apply(this, args);  // that calls f
        return !result;                    // and negates its result.
    };
}

const even = x => x % 2 === 0; // A function to determine if a number is even
const odd = not(even);         // A new function that does the opposite
[1,1,3,5,5].every(odd)         // => true: every element of the array is odd
```

This ```not()``` function is a higher-order function because it takes a function argument and returns a new function. 

As another example, consider the ```mapper()``` function which follows. 
  - It takes a function argument and returns a new function which maps one array to another using the function. 
  - This function uses the ```map(```) function defined earlier, and it is important you understand how the two functions are different:

```
// Return a function that expects an array argument and applies f to
// each element, returning the array of return values.
// Contrast this with the map() function from earlier.
function mapper(f) {
    return a => map(a, f);
}

const increment = x => x+1;
const incrementAll = mapper(increment);
incrementAll([1,2,3])  // => [2,3,4]
```

Here is another, more general, example which takes two functions, ```f``` and ```g```, and returns a new function which computes ```f(g())```:

```
// Return a new function that computes f(g(...)).
// The returned function h passes all of its arguments to g, then passes
// the return value of g to f, then returns the return value of f.
// Both f and g are invoked with the same this value as h was invoked with.
function compose(f, g) {
    return function(...args) {
        // We use call for f because we're passing a single value and
        // apply for g because we're passing an array of values.
        return f.call(this, g.apply(this, args));
    };
}

const sum = (x,y) => x+y;
const square = x => x*x;
compose(square, sum)(2,3)  // => 25; the square of the sum
```


