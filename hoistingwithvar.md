# WARNING

If you don't want to know more about history, just read [this](hoisting.md) to only know about how the current declarators work. Continue on if you want to learn about the mostly-unused `var`.

It warms my heart, however, that the first reader of my site has requested that I explain how `var` works. Such masochistic behaviour should normally be frowned upon, but I shall accede for pedagogical reasons.

Wen reading this, please do not hesitate to entitle yourself by asking for clarifications.

-------

# Hoisting.

Before the weirdness that is hoisting in Javascript, something more tame that exists in most? other programming languages as well: scope.

## Block-scope

If you come from another programming language, congratulations! You already understand block-scope.

For the rest, whenever you see lines of code surrounded by `{}`, it represents a block. 

`let` will declare its variable in its current block-scope, or the global scope if it is not inside a block. 

Any variables that are declared in a block may be accessed by any of its inner blocks:

```javascript
{
    const a = "Hello";
    {
        console.log(a); //displays "Hello";
    }
}
```

The Javascript interpreter will keep going one block-scope up, searching for the closest definition of `a` before returning its value. 

```javascript
//outer scope
const a = "Hello";
{ //inner scope
    const a = "World";
    console.log(a); //displays World
}
display(a); //displays Hello
```

What about when a is declared in two different block-scopes? The interpreter finds `a` in the inner scope, thus uses its value of `World`. Since the value is already found, the value of `a` in the outer scope does not matter as it will never be accessed.

`let`, `const`, and function declarations are all block-scoped. Also, from now on, when I refer to `let`, I mean both `let` and `const`, since they follow basically the same rules with regard to scopes and hoisting.

## Function scope

```javascript
//outer scope
function f() {
    //f scope
    if (true) {
        //if scope
        let a = "True";
    }
    console.log(a) // Line A: Error
}
f();
```
If you were a fast learner, you can easily see why this code is invalid. When Line A is executed, the interpreter first looks for `a` in its current scope, f scope. It doesn't exist. It then goes one scope up, into the outer scope, and looks for `a` there. It doesn't exist as well. Since the outerest scope has been reached, it throws a ReferenceError saying that `a` is undefined.

Note that it never looks into if scope for `a` since Line A was never part of the if scope.

---

What if we change from using `let` to `var`, however?

```javascript
//outer scope
function f() {
    //f function scope. a is accessible anywhere in this function!
    if (true) {
        //if scope
        var a = "True";
    }
    console.log(a) // Line B: "True"
}
f();
```

The code mysteriously outputs `"True"` now! Why?

`var` is special. Unlike `let`, `var` declares its variables in the closest _function_ scope instead. Therefore, when looking up `a`, the interpreter will first check the f function scope and see that `a` has indeed been declared and returns its value.

## Hoisting

### The basics

```javascript
const PI = 3.14;
// PI = 2; is disallowed, constants cannot be redefined.
let sum = 0;
sum += 1;
{
    var product = 1;
}
product *= 10; //remember var is function-scoped.
function square(number) {
    return number ** 2;
}
```
In essence,
- `const` declares a constant that is read only in its block scope and can never be reassigned nor redeclared with `function`, `const`, `var`, or `let`
- `let` declares a variable that can can both be read from and reassigned in its block scope. It cannot be redeclared with `let`, `const`, `var`, or `function`
- `function` declares a variable that can both be read from and reassigned in its block scope. It cannot be redeclared with `let` nor `const`, but declaring a function or `var` with the same name is allowed.
- `var` declares a variable that can both be read from and reassigned in its function scope. It cannot be redeclared with `let` nor `const`, but declaring a function or `var` with the same name is allowed even.
```javascript
function a() { 
    return 10;
}
function a() { //this is allowed
    return 20;
}
```

### Hoisting: Function Declarations

```javascript
function example1() { 
    alert(getHello()); 
    function getHello() {
        return "Hello!";
    }
    function redundantFunction() {
        return "Hello!";
    }
}
```
`getHello` is defined only after `alert(getHello());`, but `example1()` still alerts `Hello!`. This is because function declarations are *hoisted* to the top of the scope. The interpreter will move *all* function declarations in the current scope to the top of the scope as follows.

```javascript
function example1() { 
    function getHello() {
        return "Hello!";
    }
    function redundantFunction() {
        return "Hello!";
    }
    alert(getHello()); 
}
```

### Hoisting: `let` Declarations

```javascript
let a = 1;
function example2() { 
    alert(a);
    
    let a = 20;
    alert(a);
}
```

Reading the code line by line, one might expect `example2` to first alert 1 and then 20. But similar to function declarations, the left part of `let` statements are also hoisted to the top of their scope:

```javascript
let a = 1;
function example2() { 
    let a; //hoisting, note that only the declaration but not the assigment is hoisted.
    alert(a); //ReferenceError, a is not defined.
    
    a = 20; //now a is being assigned.
    alert(a);
}
```

The most important difference between function declarations and `let` statements is that the entirety of the function declaration gets hoisted to the top, whereas only the `let name` part of `let name = value;` statement gets hoisted. This is why when trying to read from `a` in `example2` there is ReferenceError stating that `a` is not defined.

### Hoisting: `var` Declarations
```javascript
var a = 1;
function examplevar() { 
    alert(a); //this returns undefined
    if (false) { //this will never be true
        var a = false;
    }
}
```
`var` is hoisted just like let. But remember that `var` is function scoped, and thus its hoisted version would look like:
```javascript
var a;
a = 1;
function examplevar() {
    var a;
    alert(a); //this alerts undefined, not ReferenceError
    if (false) { //this will never be true
        a = false; //hoist to the top of the nearest function
    }
}
```
An important difference is that trying to access a `let`ted variable after it has been hoisted but before it has been assigned will result in a ReferenceError, but doing the same for `var` results in undefined instead.
### Hoisting, altogether

```javascript
function example3() { 
    let a = 20; 
    alert(a);
    function a() { //there
        return 100;
    }
}
example3();
```
It looks like the code above should alert 20, and then raise an error due to redeclaring `a` at `there`. But firstly, function declarations are hoisted. Then, `let` and `var` statements are hoisted. So the above code would look like this after hoisting:
```javascript
function example3() { 
    function a() { //function declarations moved to the top first
        return 100;
    }
    let a; //let statements moved to the top next, error here
    a = 20; //assignment part of the let
    alert(a);
    
}
example3();
```
As can be seen now, function `a` has already been declared and so cannot be declared again, resulting in an error before the alert can happen.
