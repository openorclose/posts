# The magical `for` statement
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

The code does delay the output. But why do they all display 5 instead of 0, 1, 2, 3, 4 as intended?! 

This is because `i` is declared outside the for loop, and thus when its value is looked after 1s+ in each `console.log` call, the only scope that `i` exists in is outside the loop, and since the loop has already finished executing `i` will have a value 5 for all `console.log` calls. 

------

### An aside on setTimeout

Before that, another reader! It is the greatest honour to have my ramblings perused by yu, and to be humbled by your criticism of my naive presumptions of newcomers to the world's best programming language my worst nightmare. After all, it is my self-proclaimed duty to educate the blissful majority, and indeed I have so carelessly overlooked certain language features and their not-so-simple meanings. You leave me no choice but to right this wrong. 

Perhaps his greatest concern was how `setTimeout` worked, in particular why `setTimeout(fun, 0)` did not behave like `fun()`, running immediately and thus logging `0`. From an asynchronous point of view, yes, the idea might be hard to grasp at first, for few have been blessed like me to have grown up in a synchronous world, where only one thing happens at a time. But such is the world of Javascript. And now a personal story that hopefully explains how `setTimeout` works.

--------

On one wind peaceful and sun beautiful day, with varying numbers forlicking about in the sky prancing and looping around, I stepped with light and fast footsteps and headed to school. That's what my teacher told me to do at least, and so I did. 

My mother taught me from young to only concentrate on one thing at a time, if not the police would come and catch me. 

I did not like focusing, but neither did I like not focusing stuck inside a cell, thus no force on this Earth would ever stop me from completing my mission.

Not even when I received a new task seemingly from God. Look up immediately. I don't know if his command was lost in translation, from ancient Hebrew to modern Javascript, because in my world immediately meant the same thing as a tech support ticket marked Urgent. Relax, the subconscious in me prayed, I got this.

A negligible amount of time later, I was finally in school. No time to dally though, and I carried out my next set of instructions.

I looked up. Just as heaven mandated. What, I amenned. It was just a lonely 5 floating above me in the sky. Big deal. And to think God created the heavens just to have a 5 in it. I mean I _was_ 5 once, does that make me God? 

I had no more items on my agenda, and so took the chance to venture into the unknown. Making friends. 4 of them, to be precise. Short or long or whatever they appeared to be, all of them had one thing in common. They, like me, had received the exact same words from God but each mysteriously to only do so one second apart from another. But they, like me, were also told by their mothers that they had to concentrate on one thing at a time, if not the police would come and catch them. 

And so after finishing up what they were doing in a negligible amount of time, and after their required waiting period, when they all so dutifully looked up into the once diverse sky containing a multitude of numbers, only the singleton 5 remained. 

Who knew that the spectacular parade of numbers of all numerators and denominators had taken place while we were busy.

Who knew that digits had the capability to give up on waiting for people to look them up.

Who knew that after the fireworks of incrementation, only one victorious 5 would remain, the others vanquished in addition, never to grace anyone as God had originally intended.

And so, this is why when we look up the value of `i` after all is said and done it is not what it's supposed to be.

------

This is why we put the declaration inside the for loop, so it works as intended:

```javascript
for (let i = 0; i < 5; i++) {
    setTimeout(() => console.log(i), i * 1000);
}
```

But why does this work? What value of `i` does each alert refer to when being called? The answer is that ES2015 does a bit of magic to ensure that these type of callbacks that are frequently used work as people imagine it to.

(Edit: My sincerest apologies. I previously thought that there was no way to present the copying of i into the for body scope in pure Javascript. But just like Javascript makes mistakes, I make them too. Thankfully I discovered this myself. (Mostly because I think only one other person has bin reading this multi-parted love letter and he doesn't suck up to Javascript enough. Does his ignorance of Javascript pain me more than the potential embarrassment he may have delivered? That's a story for another day.) I am thusly spared from bowing down to another being pointing out my faults. Phew. A glimpse into my shameful past is always available courtesy of version control, should you be the type of person who enjoys partaking in schadenfreude.)

For loop with declaration of variables converted into a for loop without declaration in the `for` parentheses.
**WARNING: This only applies to variables declared with `let` and `const`. For `var` no matter where you declare the variables they'll always be hoisted up to the nearest function scope remember? Check out [scopes.md](scopes) if you don't.**
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
