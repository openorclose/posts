# Functions: The Holy Trinity

A long long time ago, there were only 

| Properties \ Function type | Declaration | Expression  | Arrow |
| -------------------------- | ----------- | ----------- | ----- |
| Named?                     | Yes         | Your choice | No    |
| Expression?                | No          | Yes         | Yes   |
| `this`                     | Own         | Own     | Inherited |


## Function Declarations

This is probably the first type of functions that learners of Javascript will be introduced to, especially when coming from Java or C.

```javascript
function functionName(param1, param2, ..., paramN) {
  statements;
  return expression;
}
```

And then they will learn that this code creates a function named `functionName` that can optionally do stuff with parameters and subsequently return a value. (If no return value is specified the function will be treated as if its last line was `return undefined` and return `undefined`.) (Also, function declarations are [hoisted](hoisting.md) up to the top of its block scope, whereas function expressions and arrow functions are just that, expressions, and are treated the same as you would treat any other expression like `42`, `alert`, `"a string"`, `null`, etc etc and do not get hoisted anywhere.)

## Function Expressions

Looks can be deceiving. And they certainly are especially for newcomers to javascript. Function expressions can look eerily similar to function declarations:

```javascript
const functionExpression = function namedFunctionExpression(param1, param2, ..., paramN) {
  statements;
  return expression;
}
```

Ignoring the constant declaration, the _expression_ on the right is exactly the same as a function declaration that declares a function named `namedFunctionExpression`! 


## Arrow Functions

```javascript
// general case
(param1, param2, ..., paramN) => {
  statements;
  return expression;
};

// only one parameter, parantheses can be removed: 
oneparam => {
  statements;
  return expression;
}

// the body contains only one expression or returns only one expression:
(param1, param2, ..., paramN) => expression
// the above is the same as: 
(param1, param2, ..., paramN) => {
  return expression;
}
```

Freshly introduced since ES2015, the arrow function syntax is much more concise than its function expression counterpart. But do take note of some the features that arrow functions lack in compared to function declarations and expressions. Firstly, they cannot be named, and so they cannot be referred to without assigning it to a variable beforehand. This is a concern if recursion is required and certain functional programming knowledge will be required to cretate an arrow function that can call itself.

Also, the arrow function does not provide its own `this` value, instead using the `this` value from the outer block scope when referred to from inside the arrow function. This means that when creating objects that need to refer to `this` constantly, the more verbose function expression syntax will be required.

```javascript
const object = {
  property: "a property",
  //method: () => this.property //this will result in an error as this refers to the global scope and not object
  method: function() {
    return this.property;
  }, // have to use the longer function expression syntax instead for proper this
  method() {
    return this.property;
  } // or if you want to refer to the proper this and have a shorter syntax, this shorthand is really good.
}
```

