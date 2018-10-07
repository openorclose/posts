# Magic of the `for` statement post ES2015
First things first. A `for` statement in ES2015: 
```javascript
let i = 0; // yes, i is normally declared in the for, but just wait.
for (i = 0; i < 5; i++) {
    console.log(i);
}
```
The above code correctly logs 0 to 4 in order. But what if we want to wait one second between each `console.log`? We might try :
```javascript
let i = 0; // ditto
for (i = 0; i < 5; i++) {
    setTimeout(() => console.log(i), i * 1000);
}
```
Output:
```

> 5
(wait 1s)
> 5
(wait 1s)
> 5
(wait 1s)
> 5
(wait 1s)
> 5
```

The code does delay the output. But they all display 5 instead of 0, 1, 2, 3, 4 as intended!?

This is because `i` is declared outside the for loop, and thus when its value is looked after 1s+ in each `console.log` call, the only scope that `i` exists in is outside the loop, and since the loop has already finished executing `i` will have a value 5 for all `console.log` calls. 

This is why we put the declaration inside the for loop, so it works as intended:

```javascript
for (let i = 0; i < 5; i++) {
    setTimeout(() => console.log(i), i * 1000);
}
```

But why does this work? What value of `i` does each alert refer to when being called? The answer is that ES2015 does a bit of magic to ensure that these type of callbacks that are frequently used work as people imagine it to.

(Edit: My sincerest apologies. I previously thought that there was no way to present the copying of i into the for body scope in pure Javascript. But just like Javascript makes mistakes, I make them too. Thankfully I discovered this myself. (Mostly because I think only one other person has bin reading this multi-parted love letter and he doesn't suck up to Javascript enough. Does his ingorance of Javascript pain me more than the potential embarrassment he may have delivered? That's a story for another day.) I am thusly spared from bowing down to another being pointing out my faults. Phew. A glimpse into my shameful past is always available courtesy of version control, should you be the type of person who enjoys partaking in schadenfreude.)

For loop with declaration of variables converted into a for loop without declaration in the `for` parentheses.
**WARNING: This only applies to variables declared with `let` and `const`. For `var` no matter where you declare the variables they'll always be hoisted up to the neearest function scope remember? Check out [scopes.md](scopes) if you don't.**
```javascript
{
    let i;
    for (i = 0; i < 5; i++) {
        const copyOfI = i; // first we get a copy of i...
        // note that we cannot redeclare i here (it is done below instead)
        // because we want to copy copyOfI back into i in the end
        // and if we do redeclare i here we cannot access the original i...
        { // so we create a new block here so that we can redeclare i.... 
            let i = copyOfI; // finally we can redeclare i here...
            // also note that we cannot just dump the loop of the body here
            // instead of another scope below.
            // because if we did then it would be impossible to declare a new i 
            // in the body. (why someone would do that is out of my understanding though, but you can.)
            { // new block for the body of the loop so that people can declare another i if they are crazy
                // LINE A: const i = "I don't care about the loop i, because i am more important!" //This is possible
                setTimeout(() => alert(i), i * 1000); //If you uncomment the previous line i would be that string above.
            }
            copyOfI = i; // we copy the value of i back into the copyOfI.
            // note that if someone /did/ redeclare i in LINE A, the i in 
            // this scope would be unchanged, and will not screw up the loop.
        }
        i = copyOfI; // finally. put copyOfI back into i. finally.
    }
}
```

As to why the interpreter needs to copy the copied version of `i` back into the original `i`, here is why: 
```javascript
let count = 0;
for (let i = 0; i < 5; i++) {
    i = 100;
    count++;
}
alert(count); //expecting 1
```
Clearly the code should terminate after one iteration, and that's what the copy back ensures.

The next time you use the for loop, think again if you deserve such special treatment. Going through all that for your benefit with no reward in return, yet Javascript provides so willingly. Count your blessings you all.
