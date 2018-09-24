# Intricacies of the `for` statement
First things first. A `for` statement in ES2015: 
```javascript
let i = 0; // we could declare i in the for loop, but just hold on
for (i = 0; i < 5; i++) {
    alert(i);
}
```
The above code correctly alerts out 0 to 4 in order. But what if we want to alert them with a 1s interval between each? We might try :
```javascript
let i = 0; // we could declare i in the for loop, but just hold on
for (i = 0; i < 5; i++) {
    setTimeout(() => alert(i), i * 1000);
}
```
The above code does correctly alert at intervals of 1s between. But they all alert 5 instead of 0, 1, 2, 3, 4 as intended! This is because `i` is declared outside the for loop, and thus when its value is required after 1s+ in each alert call, the loop has already finished executing and `i` will be 5 for all alerts. 

This is why we put the declaration inside the for loop, so it works as intended:

```javascript
for (let i = 0; i < 5; i++) {
    setTimeout(() => alert(i), i * 1000);
}
```

But why does this work? What value of `i` does each alert refer to when being called? The answer is that ES2015 does a bit of magic to ensure that these type of callbacks that are frequently used work as people imagine it to.

```javascript
for (let i = 0; i < 5; i++) {
    interpreter: let i = copy of original i;
    {//new block for the body of the loop
        setTimeout(() => alert(i), i * 1000);
    }
    interpreter: copy the newly copied version of i back into the original i;
}
```

The interpreter creates a completely new copy of `i` at each iteration for the body of the loop to use, so that it has its own `i` to refer to if needed after the loop has finished executing. Note that it is impossible to represent what the interpreter does perfectly in ES2015 code, and so I had to explain it in words.

Also note the new block that is created. This is because one can actually declare `i` again in the body of the loop; it is just another scope after all!
```javascript
let count = 0;
for (let i = 0; i < 5; i++) {
    let i = "Random stuff";
    count++;
}
alert(count) //expecting 5
```
And so the above code would still run.

And lastly, as to why the interpreter needs to copy the copied version of `i` back into the original `i`, here is why: 
```javascript
let count = 0;
for (let i = 0; i < 5; i++) {
    i = 100;
    count++;
}
alert(count); //expecting 1
```
Clearly the code should terminate after one iteration, and that's what the copy back ensures.
