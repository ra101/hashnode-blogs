---
title: "Quine(s) in JavaScript"
datePublished: Fri Feb 12 2021 19:51:11 GMT+0000 (Coordinated Universal Time)
cuid: ckvp89i6r09wkj3s11hbq4vxf
slug: quines-in-javascript-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1636289067430/s3htDtVNG.png

---

[Quine](https://en.wikipedia.org/wiki/Quine_(computing)) is a program that takes no input but outputs a copy of its own code. Unlike other languages writing a quine in JavaScript/NodeJS is quite easy.

<br />
<br />

**Here we go!**

```Javascript
function quine() { console.log(quine.toString()) }
```

The key here is that any function in JavaScript can be converted into a string and can be printed. Also, `console.log` is not the only option, `alert` can be used too. Although it will not work in a node terminal.

**Above is a function that prints its source code but it is not a file that can be executed.** So let's add a Call statement,

```js
function quine() { console.log(quine.toString()+" quine();") } quine();
```

Note that we needed to add something extra in the log statement to achieve our goal. And `;` was probably not needed.

<br />

Let's make it a little elegant, We know that JavaScript can make a function run as soon as it is defined, By using an IIFE ([Immediately Invoked Function Expression](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)).

```javascript
( function quine() { console.log("( " + quine.toString() + " )()") } )()
```

Note that we manipulated the log statement as required.

For some more Quines in NodeJS: https://catonmat.net/quine-in-node

<br />

**Now let's take [Arrow-Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) and [Format-Strings](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Text_formatting) into this equation and It becomes almost eye-blindingly beautiful!**

```Javascript
($=_=>`($=${$})()`)()
```

To Understand let's remove IIFE and extra parentheses in the format-string. And also, add some spacing.

```Javascript
$    =    _    =>    `$=${$}`
```

So, the first `$` is a variable that contains an arrow function.
`_` is a random parameter for the arrow function which remains unused.
After the arrow, this is our format-string which can be divided into 2 parts, the String, "$=", and the Variable which is first `$` itself. 

<br />

**Lastly, A Quine needs to be executable but it doesn't mean that programs resulting in errors can't be a Quine. Here is an example**

```Javascript
throw 0
^
0
```

Link: https://github.com/nmrugg/quine

This program when executed as a .js file with the help of NodeJS outputs its own source code. 

How this works is, NodeJS returns an error at the first line, the rest of the code is how the error looks like.

If you make your own JS Quine or you want to share Quine in other programming languages, then please do write in the comment section.

