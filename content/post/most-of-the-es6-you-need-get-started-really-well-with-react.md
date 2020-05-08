---
title: The 20% Of ES6 you end up using 80% of the time - Let & Const.
date: 2019-06-02T23:27:34+03:00
tags: ["React", "Javascript","ES6","Coding"]
draft: false
---
## Variable declaration with let & const
>  This is the first part in the series. I will be covering the other sections in part 2.

One of the more confusing part of learning a Javascript framework is that everything looks like magic at first. Especially when your knowledge of modern Javascript is not up to date.
You are unsure which part of the code is some **`abracadabra`** or just plain ES6 Javascript. At least this was my feeling when I started learning React,or even Angular(although that was Typescript).You get caught thinking what the hell is this *`bind(this)`* everywhere and why, what of those weird looking syntaxes and these funny variable assignments too...oh that was `destructuring`!.

<br>

<br> <br> <br>


This series is an attempt to layout some ES6 concepts I which I knew thoroughly before delving into React or any modern Javascript software for that matter.These fundamentals will make you a more productive modern Javascript developer quickly regardless of framework.

This series is geared towards  beginners who already have some Javascript knowledge.

We'll be taking a dive at the underlisted ES6 concepts.These I believe are essential to getting a quick grasp of React in particular much faster. Some are more simpler that the other and you'll probably know them already. But, I guess it's fine to go over them nonetheless.

I have divided the series into two sections. This is the first section consisting of 5 parts

> You can skip to any topic by clicking on them below :

* [Ternary Operators](https://olajohn.me/post/es6-ternary-operators/)
* [Template literals (Template strings)](https://olajohn.me/post/es6-template-literals/)
* [ES6 Module system](https://olajohn.me/post/es6-module-system/)
* [Destructuring assignments](https://olajohn.me/post/es6-destructuring/)

### Variable declaration with let/const :heart_eyes:
Before ES6 ``var`` was the de facto keyword for declaring variables. With ES6, two new keywords *`let`* and *`const`* were introduced for variable declarations. But what are the differences and why bother with them anyways?
There are a couple of gotchas with declaring variables with the *`var`* keyword. *`Scope`* and *`hoisting`* were chief amongst the issue with **`var`**. But what does these 2 even mean?


> The ***scope*** of *`var`* is the entire function block in which it is defined.It is also available globally when declared outside a function. *`let`* on the other hand is limited to the block scope in which it was defined.

* **Global Object :confounded:**

{{< highlight jsm "linenostart=1" >}}
var testOfVar = "I am 'Var'";
let testOfLet = "I am 'Let'"

console.log(this.testOfVar) // Outputs I am 'var'
console.log(this.testOfLet) // Outputs undefined
{{< / highlight >}}
As you would see the variable declared with `let` is not attached to the global object and therefore not accessible globally.On the other hand `var` declared variable is accessible globally and therefore can be accidentally altered anywhere within the program.This is really bad and can be cause of debugging frustrations.Hence, it is recommended to use `let` instead of `var` almost always.

* **Scope :smirk:**

> The **`let`** statement declares a block scope local variable, optionally initializing it to a value.

In simple english, variables declared with `let` has a limited scope within the `block` it is declared. You can initialize the variable optionally if you desire. But wait, what is a *`block scope`* ?,A **block scope** is the area within `if`, `switch` conditions or `for` and `while` loops. Generally speaking, whenever you see `{...expression}`, this is a block. Unlike `var` where the variable is hoisted to the top of the function regardless of the **block** scope. Hence:

**var** declared variable is accessible throughout the entire function scope and can be modified.Why is this such a big deal you ask?, Some code snippets to the rescue below:


{{< highlight jsm "linenostart=1" >}}
function testOfVar() {
 var myName = "John" // this will later be modified within the if block.
 if (true) {
   var myName = "Joshua"
   console.log(myName) // this is same variable as the first 'myName'
                       // scope is ignored.Modifies the variable and hoist it to top of function.
                       // Outputs 'Joshua'
 }
 console.log(myName) // myName has been modified within the if block.
                    // Every reference of myName within this function block now refers to "Joshua"
}

function testOfLet() {
 let myName = "John" // this will not be modified
 if (true) {
   let myName = "Joshua"
   console.log(myName) // this is treated as another variable. The scope is limited to this if block.
                       // scope is respected, this variable will not modify anything outside this block
                       // Outputs 'Joshua'
 }
 console.log(myName) // myName declared at the top of the function is modified from the if block.
                     // Output 'John'
}

{{< / highlight >}}


* **Re-Declaration :see_no_evil:**

With `var` it was possible to re-declare a variable which isn't a good thing.You could easily re-declare a variable that has already been declared and assign a different value to it. It will introduce a bug without you noticing. `Let` throws an error when you try to re-declare a variable within the same scope. Example below:

{{< highlight jsm "linenostart=1" >}}
function testOfVar() {
 var myName = "John";
 var myName = "Joshua" // this doesn't even make sense, but var makes it possible.
 console.log(myName)  // Outputs 'Joshua'
}

function testOfLet() {
 let myName = "John";
 let myName = "Joshua" //
 console.log(myName) // SyntaxError 'myName' is already been declared
}
{{< / highlight >}}

> *In summary avoid using the `var ` statement for variable declaration. You should almost always pick the `let` keyword when you need to declare a variable*.

**What about `const`? :smile:**

*`const`* is quite similar to *`let`*. The major difference is that *`const`* can not be re-assigned.Do not confuse re-assigning with re-declaring. In most cases you'd want to declare a variable and make some re-assignments depending on certain conditions. Let's look at an example:
{{< highlight jsm "linenostart=1" >}}
function constTest() {
 const age = 6
 const ageDeclaration = ""
 if (age > 5) {
   ageDeclaration = "I am " + age + " years old" // TypeError: Assignment to constant variable
 }
 return ageDeclaration
}

// But this is possible
function constModifier() {
 const myProfile = { age: 20, name: "John", ageDeclaration: " " }
 if (myProfile.age > 5) {
   myProfile.ageDeclaration = "I am " + myProfile.age + " years old" // the ageDeclaration property of my profile can be re-assigned
 }
 return myProfile // returns { age: 20, name: 'John', ageDeclaration: 'I am 20 years old' }
}

{{< / highlight >}}

In the example above we have declared `ageDeclaration` with an empty string. Within the `if` block we are re-assigning it to keep track of the `age` variable. Note when we re-assigned the `ageDeclaration` within the `if` block, we did so without using the ``let`` statement. This is the basic difference between re-declaration and re-assigning.Almost always, what we intend to do is to re-assign a variable not re-declare it. However, with ``const`` you can neither re-declare or re-assign.Although, it is possible to change the properties of the variable declared with ``const``.

> *Use `const` when you are sure there is no intention to re-assign the variable. However, you can still use ``const`` for objects,arrays,classes etc where some of the properties/members has to be re-assigned/modified. Refer to code example above!*


<iframe width="560" height="315" src="https://www.youtube.com/embed/YN3sULAn2SA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

 **Next Up in Section 1**

* [Ternary Operators](https://olajohn.me/post/es6-ternary-operators/)
* [Template literals (Template strings)](https://olajohn.me/post/es6-template-literals/)
* [ES6 Module system](https://olajohn.me/post/es6-module-system/)
* [Destructuring assignments](https://olajohn.me/post/es6-destructuring/)


> *Coming up next in section 2! : Not available yet*

* [High-order Functions](#high-order-functions)
* [Arrow functions](#arrow-functions)
* [Map, Filter and Reduce](#map-and-filter)
* [Spread/Rest Operator](#spread-rest-operator)
* [Default function parameters](#default-function-parameters)
* [ES6 Classes](#es6-classes)
* [Async JavaScript](#async-javascript)




