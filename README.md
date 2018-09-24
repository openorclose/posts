# Javsacript: In-depth Workings of Basic Features
Javascript has touched the hearts of many (myself included), and thus it is to my great disappointment that there are people out there who fail to grasp its true beauty. To them, it is but a tool to add simple dynamism to a website, some code to copy and paste to generate ad revenue, or worse; they project their preconceptions about how _other_ programming languages work onto Javascript, and then have the gall to complain about how their perfect code does not run as they intended. 

If you just want the TLDR of certain behaviour and not listen to my stack overflowing preambles, follow these links (but I will hate you):
- [Scopes and Closures in Javascript](scopesclosures.html)
- [Hoisting](hoisting.html)
- [The For Statement](for.html)

If not, and you want to know the history and motivations behind these wonderful quirks of Javascript, follow these instead (and I will love you):

- [Scopes and Closures in Javascript (In depth)](indepth/scopesclosures.html)
- [Hoisting (In depth)](indepth/hoisting.html)
- [The For Statement (In depth)](indepth/for.html)

Or perhaps you would like to first know about some brief Javscript history? Then it is my honour if you choose to read on.
# A Brief History of Javascript 
## Why Javascript looks like Java (but isn't like Java at all)
| Java          | Javascript    |
| ------------- |---------------|
| Strongly typed| Weakly typed  |
| Compiled      | Interpreted   |
| Bad           | Good          |

The table of their differences goes on forever, being similar in only one aspect: syntax. 

Brendan Eich, the creator of Javascript, was tasked to allow embed Scheme into Netscape Navigator, but Netscape also had a collaboration with Sun, where they decided to include Java in their browser as well. So, they wanted this new language to have Java-like syntax as well. And so it did.

## The unlucky 4 of Chinese culture

Javasript was standardised some time ago by Ecma, a standards organisation. They released versions 1, 2, and 3 of ECMAScript in 1997, 1998, and 1999 respectively. It then took 10 years for ES4 to be abandoned and ES5 to be released in 2009. 

The Chinese have a rich history, more so than even Javascript, that I have to admit, and since the number 4 sounds like death in their language, it is considered inauspicious. 

The moral here is to always listen to your elders and so they should never have tried to come up with an ES4 in the first place, and instead skipped to ES5.

But like rebellious little children, they tried. 10 years and countless politics later, they reached a compromise to at least try and fix some bugs in ES3 first, releasing ES5 that did just that in 2009, before continuing on with the squabble.

It took a long while, but some features originally intended to be in ES4 managed to be agreed upon and were released in ES6. It was at this point that someone had a genius idea to name versions by year from that point on, since ES13 would be coming a few years later and they wanted to avoid another calamity. 

I pray to all gods of all religions and the no-god of atheism that no natural disaster shall befall Javscript again.

Amen.

## Javascript, mostly backwards-compatible

It is safe to say that one of the requirements for perfection is imperfection. 

The `==` operator is so well known and well laughed at by everyone that I did not bother explaining it. Just use `===`. 

The `==` operator has a million rules to remember, and a possible motivation of such insanity was to have programming amatuers more willing to take a step into the language and start creating dynamic pages.

For example, a simple game where a user had to guess the mystery number:

```javascript
// Days of this code are long gone thankfully
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

Ok, perhaps it's not that simple since Netscape and Internet Explorer both did their own thing for a while. But the point is, the value of an input field is a string, but the mystery number is a number. To "make things easier", the interpreter would try and convert one side or the other to get an equality. Eventually, it was realised that the `==` operator was a bad idea, and thus `===` the strict equality operator was introduced. Note that the `==` was never redefined. This was a conscious decision to not break archaic code on the web, and this reason still continues to dictate how future versions of Javascript are defined.






