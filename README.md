# Javsacript: In-depth Workings of Basic Features
Javascript has touched the hearts of many (myself included), and thus it is to my great disappointment that there are people out there who fail to grasp its true beauty. To them, it is but a tool to add simple dynamism to a website, some code to copy and paste to generate ad revenue, or worse; they project their preconceptions about how _other_ programming languages work onto Javascript, and then have the gall to complain about how their perfect code does not run as they intended. 

If you just want the TLDR of certain behaviour and not listen to my stack overflowing preambles, follow these links (but I will hate you): 
- [Scopes and Closures in Javascript](scopesclosures.md)
- [Hoisting](hoisting.md)
- [The For Statement](for.md)

If not, and you want to know the history and motivations behind these wonderful quirks of Javascript, follow these instead (and I will love you):

- [Scopes and Closures in Javascript (In depth)](indepth/scopesclosures.html)
- [Hoisting (In depth)](indepth/hoisting.html)
- [The For Statement (In depth)](indepth/for.html)

Or perhaps you would like to first know about some brief Javscript history? Then it is my honour to have you read on.
# A Brief History of Javascript (Personally Filtered by Me)
## Why Javascript looks like Java (but isn't like Java at all)


| Java          | Javascript    |
| ------------- |---------------|
| Strongly typed | Weakly typed  |
| Compiled      | Interpreted   |
| Bad           | Good          |

The table of their differences goes on forever, being similar in only one aspect: syntax. 

Brendan Eich, the creator of Javascript, was tasked to embed Scheme into Netscape Navigator, but Netscape also had a collaboration with Sun, where they decided to include Java in their browser as well. So, they wanted this new language to have Java-like syntax as well. And so, Javascript inherited Java's syntax, thankfully without its bad parts.

## The unlucky 4 of Chinese culture

Javasript was standardised some time ago by Ecma, a standards organisation. They released versions 1, 2, and 3 of ECMAScript in 1997, 1998, and 1999 respectively. 

The Chinese have a rich history, more so than even Javascript, that I have to admit, and since the number 4 sounds like death in their language, it is considered inauspicious. 

The moral here is to always listen to your elders and so they should never have tried to come up with an ES4 in the first place, and instead skipped to ES5.

But like rebellious little children, they tried. 10 years and countless politics later, they reached a compromise to at least try and fix some bugs in ES3 first, releasing ES5 that did just that in 2009, before continuing on with the squabble.

It took a long while, but some features originally intended to be in ES4 managed to be agreed upon and were released in ES6. It was at this point that someone had a genius idea to name versions by year from that point on, since ES13 would be coming a few years later and they wanted to avoid another calamity. 

I pray to all gods of all religions and the no-god of atheism that no natural disaster shall befall Javscript again.

Amen.

----------

## Not breaking the web is a heavy burden to carry

It is safe to say that one of the requirements for perfection is imperfection. 

Even more so when the imperfection is openly recognised and not hastily patched up.

Because patching up a language is easy, but patching up human laziness is not.

### The == operator

The `==` operator is so well known and well laughed at by everyone that I did not bother explaining it. Just use `===`. 

The `==` operator has a million rules to remember, and a possible motivation of such insanity was to have programming amatuers more willing to take a step into the language and start creating dynamic pages.

For example, a simple game where a user had to guess the mystery number:

```javascript
// Days of this are long gone thankfully
if (document.getElementById) {
    var inputField = document.getElementById("inputfield")
} else if (document.all) {
    var inputField = document.all.inputfield
} else if (document.layers) {
    var inputField = document.inputField
} else {
    //too bad? later will have NullPointerException or segmentation fault.
}
alert(inputField.value == 12345)
```

Ok, perhaps it's not that simple since Netscape and Internet Explorer both did their own thing for a while. But the point is, the value of an input field is a string, but the mystery number is a number. Probably to "make things easier", the interpreter would try and convert one side or the other to get an equality. Eventually, it was realised that the `==` operator was a bad idea, and thus `===` the strict equality operator was introduced. Note that `==` was never redefined. This was a conscious decision to not break archaic code on the web, and this reason still continues to dictate how future versions of Javascript are defined. 

### Automatic Semicolon Insertion

Also noteworthy in the above example is the lack of semicolons, and why `inputField` can be accessed when it is supposedly only defined within the `if` statements. 

Javascript, like with the `==` operator, will try to guess where a semicolon should be and insert it in for you. Of course there are times when results are unexpected, especially if you come from a C background where the wrong style guide is used: 

```javascript

function getObject()
{
    return //Javascript guesses that there should be a semicolon here, 
    {
        key: "value"
    }; // the semicolon here could have been included or not, it doesn't matter
}
```

Above, `getObject()` would _not_ result in any error at all, and instead give `undefined`. 

It will probably never be removed from the language, as it might break old sites. So please do use the correct style for Javascript. 

### Introduction of new reserved words in ES2015

Javascript introduced [block scope](blockscope.md) in ES2015. Not by changing `var`, as usual, but by the introduction of a new keyword `let`. `let` was never one of the reserved keywords pre-ES2015, so defining a variable named `let` like `var let = "let";` was definitely possible. What happens if you do exactly that in 2018? `let` is a reserved keyword, so an error should rightfully be thrown no?

Alas, remember that Javascript does not like for tabled sites of the past to stop working. So the new keywords that were introduced (such as `let`) are only half-reserved. They can still be used as variable names and identifiers without causing an error. (Please don't do this though.)

### Possible uture explicit tail call optimisations

ES2015 has proper automatic tail call optimisation in its specifications, but as of 2018, only Safari supports it. Chrome even removed support for it many versions back, and instead backed a proposal to introduce optmisations only when _explicitly_ requested:

```javascript
function factorial(n, acc = 1) {
    if (n <= 1) {
        return acc;
    } else {
        return continue factorial(n - 1, n * acc); //note the use of continue to signify a tail call
        //above line will /never/ be valid pre-ES2015
    }
}
```

While this proposal has been abandoned for 2 years, you can see why a reserved keyword from pre-ES2015 `continue` was used after `return` to mark a tail call. `return continue ...` was never valid syntax, and so would never break old code. If they had instead created a new keyword such as 'returntail' for example:

```javascript
function getTail(animal) {
    returntail(animal);
}
```

It would be valid valid syntax now that gets the tail of the animal if function `returntail` had been implemented, but if this flawed proposal were to be implemented, code like the above would be ambiguous to us, and the interpreter would probably throw an error about how animal is not a function and cannot be `returntail`ed.

----------
