---
title: "20% of the ES6 you will use 80% of the time while learning React."
date: 2019-06-02T23:27:34+03:00
tags: ["React", "Javascript","ES6","Coding"]
draft: false
---

One of the more confusing part of learning a Javasript framework as a newbie is that everything looks like magic at first.
You are unsure which part of the code is some ***abracadabra*** or just plain Javascript.At least this was my feeling when I started learning React.You get caught thinking what the hell is this `bind(this)` everywhere and why, what of those weird looking syntaxes and these funny variable assignments too...oh that was `destructuring`!.

This is an attempt to layout some ES6 concepts I which I knew throughly before delving into React or any modern Javascrpt software for that matter.These fundamentals will make you a more productive modern Javascript developer quickly regardless of framework.

We'll be taking a dive at the underlisted ES6 concepts.These I believe are essential to getting a quick grasp of React in particular much faster. Some are more simpler that the other and you'll probably know them already.But, I guess it's fine to go over them nonetheless.

>  **This is a pretty long post, you can skip ahead to any of the sections by clicking on their links below.**.


This is the first part in the series. I will be covering the other sections in part 2.

* [Varibale declaration with let/const](#variable-declaration-with-let-const)
* [Template literals (Template strings)](#template-literals-template-strings)
* [Ternary Opearors](#ternary-operators)
* [ES6 Module system](#es6-module-system)
* [Destructuring assignments](#destructuring-assignments)

> **Coming up next in Part 2! : Not availabe yet**

* [High-order Functions](#high-order-functions)
* [Arrow functions](#arrow-functions)
* [Map, Filter and Reduce](#map-and-filter)
* [Spread/Rest Operator](#spread-rest-operator)
* [Default function parameters](#default-function-parameters)
* [ES6 Classes](#es6-classes)
* [Async JavaScript](#async-javascript)




### Variable declaration with let/const
Before ES6 ``var`` was the defacto keyword for declaring variables. With ES6, two new keywords ```let``` and ```const``` were introduced for variable declarations. But what are the differences and why bother with them anyways?
There are couple of gotchas with declaring variables with the ```var``` keyword. ```Scope``` and ```hositing``` were cheif amongst the issue with ```var```. But what does these 2 even mean.


> The ***scope*** of ```var``` is the entire function block in which it is defined.It is also available globally when declared outside a function. `let` on the other hand is limited to the block scope in which it was defined.

* **Global Object :confounded:**

{{< highlight jsm "linenostart=1" >}}
var testOfVar = "I am 'Var'";
let testOfLet = "I am 'Let'"

console.log(this.testOfVar) // Outputs I am 'var'
console.log(this.testOfLet) // Outputs undefined
{{< / highlight >}}
As you would see the variable declared with `let` is not attached to the global object and therefore not accesible globally.On the otherhand `var` declared variable is accessible globally and therefore can be accidentally altered anywhere within the program.This is really bad and can be cause of debugging frustrations.Hence, it is recommended to use `let` instead of `var` almsot always.

* **Scope :smirk:**

> The **`let`** statement declares a block scope local variable, optionally initializing it to a value.

In simple english, variables declared with `let` has a limited scope within the `block` it is declared. You can intilaize the variable or not. But wait, what is a `block scope` ?,A **block scope** is the area within `if`, `switch` conditions or `for` and `while` loops. Generally speaking, whenever you see `{...expression}`, this is a block. Unlike `var` where the variable is hoisted to the top of the function regardless of the **block** scope. Hence:

**var** decalred variable is accessible throughout the entire function scope and can be modified.Why is this such a big deal you ask?, Some code snippets to the rescue below:


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
                     // Every reference of myName within this function block now reffers to "Joshua"
}

function testOfLet() {
  let myName = "John" // this will not be modified
  if (true) {
    let myName = "Joshua"
    console.log(myName) // this is treated as another variable. The scope is limited to this if block.
                        // scope is respected, this variable will not modify anyting outside this block
                        // Outputs 'Joshua'
  }
  console.log(myName) // myName declared at the top of the function is modified from the if block.
                      // Output 'John'
}

{{< / highlight >}}


* **Re-Declaration :see_no_evil:**

With `var` it was possible to re-declare a variable which isn't a good thing.You could easily re-declare a variable that has already been declared and assign a different value to it. It will introduce a bug without you noticing. `Let` throws an error when you try to re-declare a varibale within same scope. Example below:

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

`const` is quite similar to ``let``. The major difference is that ``const`` can not be re-assigned.Do not confuse re-assigning with re-declaring. In most cases you'd want to declare a variable and make some re-assignments depending on certain conditions. Let's look at an example:
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

In the example above we have declared `ageDeclaration` with an empty string. Within the `if` block we are re-assigning it to keep track of the `age` variable. Note when we re-assigned the `ageDeclaration` within the `if` block, we did so without using the ``let`` statement. This is the basic difference between re-declaration and re-assigning.Almost always, what we intend to do is to re-assign a variable not re-declare it. However, with ``const`` you can neither re-declare or re-assign.Although, it is possible to change the properties of the variable decalred with ``const``.

> Use `const` when you are sure there is no intention to re-assign the variable. However, you can still use ``const`` for objects,arrays,classes etc where some of the properties/members has to be re-assigned/modified. Refer to code example above!

### Template literals (Template strings)

String interpolation and multi-line strings are probably most of the template strings concepts you'll encounter working with modern Javascript.Template strings are declared using the back ticks.
{{< highlight jsm "linenostart=1" >}}
//ES5
let exampleString = "I am an ES5 String" // or  exampleString = 'I am a String'

//ES6
let exampleString = `I am an ES6 String`
{{< /highlight >}}

* **Multi-line strings**

You can get strings in multiple lines by using the string literals instead of the more cumbersome ES5 alternative.
{{< highlight jsm "linenostart=1" >}}
//ES5
console.log('string text line 1\n' +
'string text line 2');

//ES6
console.log(`string text line
string text line 2
string text line 3`)
{{< /highlight >}}

* **String Interpolation**

>Template literals provide an easy way to interpolate expressions into strings. You do so by using the ${...} syntax:
{{< highlight jsm "linenostart=1" >}}
//ES5
function es5String() {
  const name = "John"
  let age = "20"
  const country = "Nigerian"
  const introdcution = "My name is " + name + " I am a " + age + "year old " + country
  console.log(introdcution)
}

//ES6
function es6String() {
  const name = `John`
  let age = 20
  const country = `Nigerian`
  const introdcution = `My name is ${name} I am a ${age}old ${country}. I will be ${age + 5} years old in 5 years`
  console.log(introdcution)
}
{{< /highlight >}}

> *Template strings are commonly used with styled-components library in React*
{{< highlight jsm "linenostart=1" >}}
//Styled-Component exmaple
const fontSize = calculateFontSize();
RedCenteredText = styled.h1`
	color: red;
	text-align: 'center;
	font-size: ${fontSize}`
{{< /highlight >}}

### ES6 Module system

ES6 modules are one of the more critically important concept you will come across in every React program. I will not delve in-depth as to why the module patterns is important. You can read more about that [here](https://exploringjs.com/es6/ch_modules.html), [here](https://www.freecodecamp.org/news/how-to-use-es6-modules-and-why-theyre-important-a9b20b480773/) and [here](https://www.sitepoint.com/understanding-es6-modules/). Instead I will provide a quick overview of what they are and how to quickly get started with them.

* **What are ``ES6 Modules`` ?**

> The word **"modules"** refers to a small units of independent reusable code. They are foundation of many design patterns and are neccesary to build any substantially complex application.

Let's look at buiding a computer for example, it is consisted of several parts, but you could just assemble each part without worrying about the engineering complexities behind each components or having to create your own components everytime. As Software Engineers we usually have to re-use codes either written by others or by ourselves without having to re-invent the wheel all the time.

* **Creating and Exporting Modules**

The module you are exporting could be any reusable block of code. It could be a simple variable, a function , a react component, a class, a third party module you modified etc. Let's create simple modules and understand how we can export them.ES6 provides two ways to export a module from a file: **named export** and **default export**.

Let's create some modules in a file named ``myModules.js``

> *Ignore the implementation details if you are not sure how the code works. This is one major point of using third party modules. The implementation or how it works is less important. Just know what it does and how to use it.*
{{< highlight jsm "linenostart=1" >}}
// myModules.js
const moduleConst = {title: "I am a module CONST"}

// this function takes an array and add all the numbers
const addNumberInArray = (array) => array.reduce((a, b) => a + b);

// this function takes an array and finds the average
const findArrayAverage = (array) => array.reduce((a, b) => a + b / array.length)
}
{{< /highlight >}}

Now that we have created our module, let's make them accessible to other parts of our code by **"exporting"** them.

* **Each modules can be exported individually(Named Export).**

> *With named exports, one can have multiple named exports per file. Then import the specific modules surrounded in braces. The name of imported module has to be the same as the name of the exported module.*
{{< highlight jsm "linenostart=1" >}}
// myModules.js

// individually exported
exports const moduleConst = {title: "I am a module CONST"}

// exports just this function individually
export const addNumberInArray = (array) => array.reduce((a, b) => a + b);

// exports just this function individually
export const findArrayAverage = (array) => array.reduce((a, b) => a + b / array.length)
}
{{< /highlight >}}

But that looks cumbersome and repetitive. Is there a better way?, Sure!

* **Use a single export statement for all modules in ``myModules.js``:**

{{< highlight jsm "linenostart=1" >}}
// myModules.js
 const moduleConst = {title: "I am a module comment"}

// this function takes an array and add all the numbers
 const addNumberInArray = (array) => array.reduce((a, b) => a + b);

// this function takes an array finds the average
 const findArrayAverage = (array) => array.reduce((a, b) => a + b / array.length)
}

export {moduleConst, addNumberInArray, findArrayAverage }
{{< /highlight >}}

* **Export default (Default export)**

> You can only have **one** default export per file. When the default export is imported we can then specify any name for it
{{< highlight jsm "linenostart=1" >}}
// myModules.js

// named export
 export const moduleConst = {title: "I am a module comment"}

// named export
 export const addNumberInArray = (array) => array.reduce((a, b) => a + b);

// this is the default export only 1 is allowed in this file
 export default const findArrayAverage = (array) => array.reduce((a, b) => a + b / array.length)
}
{{< /highlight >}}



* **Exporting with an alias**

You can give the exported module an alias(like a nick name), usually because you prefer a shorter name. For example the `addNumberInArray` and `findArrayAverage` could be be exported with an alias like so:

{{< highlight jsm "linenostart=1" >}}
export {moduleConst, addNumberInArray as add, findArrayAverage as avg }
{{< /highlight >}}

In summary from [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) :

> Named exports are useful to export several values from within same file. While importing, one will simply refer to the exported value by same name.

> Deafult export allows just a single default export per module.You can of course have several other named exports within same file. The default export is considered the **main** exported value. A default export can be a function, a class, an object or anything else.



* **Importing Modules**

Now that we understand how to export modules, let's look at importing them and using them. Importing is quite straightforward with the `import` keyword.
Importing from `myModules.js` will look like so:

{{< highlight jsm "linenostart=1" >}}
//app.js

import {moduleConst,addNumberInArray, findArrayAverage } from './myModules.js' //

// You can also import with an alias here if you didn't export with an alias
import {moduleConst, addNumberInArray as add, findArrayAverage as avg } from './myModules.js'

{{< /highlight >}}

 > You can import everything that is exported from a module like this:
{{< highlight jsm "linenostart=1" >}}
//app.js

import * as All from './myModules.js'

// this allows us to have access to every memeber of the module with the dot notation.Like so:

All.moduleConst
All.addNumberInArray() // since this is a fucntion we have to call it.
All.findArrayAverage()

{{< /highlight >}}

* **Importing a module with a default member**

When you import a default export , you simply use the ``import`` keyword to import the module like we've been doing. The only major difference is that you could call the import whatever name you wish. For example:
{{< highlight jsm "linenostart=1" >}}
//  myModules.js

 export const moduleConst = {title: "I am a module comment"}

 export const addNumberInArray = (array) => array.reduce((a, b) => a + b);

// this is the default export!!!
 export default const findArrayAverage = (array) => array.reduce((a, b) => a + b / array.length)

//  app.js

// all the below 3 will be referencing the `findArrayAverage` module since it is the only default export in myModules.js
import abrcadabra from  './myModules.js'
import someMagic from './myModules.js'
import findArrayAverage from './myModules.js'

{{< /highlight >}}


* **Using imported module.**

Remember, the main puporse of a module is to have a re-usable piece of code.The piece of code exposes an API to us that we can then use without having to ever understand the implementation details of the code itself. Let's start with the simple examples from `myModules`. Let's assume you have to calculate the average/sum of an array many times within your code.You could simply import ``myModules.js`` to perform the operation.Let's pretend these are even more complex algorthimic compuations and you do not want to or cannot write the functions yourself. You can simply import the module and use them in your code like so:

{{< highlight jsm "linenostart=1" >}}
//app.js
import {addNumberInArray, findArrayAverage } from './myModules.js'

const array = [1,2,3,4,5]
// I need to calculate sum of an array using the addNumberInArray function from './myModules.js'.
// Simply pass the array to the function like so:
addNumberInArray(array) //> returns the sum whhich is 15

// I need to calculate average of an array using the findArrayAverage function from './myModules.js'.
// Simply pass the array to the function like so:
findArrayAverage(array) //> returns the sum which is 3.8
{{< /highlight >}}

Let's look at more interesting example from react-router-dom.

We imported `React` from ***react*** libarary. Then imported `BrowserRouter` , `Route`, and `Link` from `react-router-dom`.

> **Note:** ***`BrowserRouter` has been imported with an alias `Router`, this is much shorter to reference throughout the code than the longer `BrowserRouter`.***

{{< highlight react "linenostart=1" >}}
//app.js

import React from "react";  // we import React from the react library
import { BrowserRouter as Router, Route, Link } from "react-router-dom" import the modules we need from the `react-router-dom`
function AppRouter() {
  return (
    <Router> // we then use the imported `Router` here
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link> // making use of the imported `Link`
            </li>
            <li>
              <Link to="/about/">About</Link>
            </li>
            <li>
              <Link to="/users/">Users</Link>
            </li>
          </ul>
        </nav>
        <Route path="/" exact component={Index} />  //making use of the imported `Link`
        <Route path="/about/" component={About} />
        <Route path="/users/" component={Users} />
      </div>
    </Router>
  );
}
export default AppRouter;

{{< /highlight >}}

### Ternary Operators
The conditional (ternary) operator is the only JavaScript operator that takes three operands. This operator is frequently used as a shortcut for the **if** statement.
The ternary operator as the word **"ternary"** implies has three main parts. The first is the **condition**, that must resolve to a boolean(truthy or falsy) values. The second part is the expression we'd like to resolve to if the `condition` is **true** and the third part is when the condtion is **false**.

**The Syntax:**


> ***(condition) ? exprIfTrue : exprIfFalse***

> *Ternary opertaor is simply a more terse version of the `if/else` statement*.

Let's make a sharp comparison with `if/else` to get a clearer picture. For an **if/else** statement you start with a condition like so : `if(..condtion)`. In the preceeding `block` you then have the expression to run if the condition is true. `else` you want the code in the **else** block to run.

{{< highlight jsm  "linenostart=1"  >}}
// if/else statement
let age = 20
if(age>=18) {
  return  "You are allowed to have a Beer"
} else {
  return "Nope"
}
// Equivalent Ternary
(age>=18) ? "You are allowed to have a Beer" : "Nope"
{{< /highlight >}}


> Besides **`false`**, possible falsy expressions are: **`null`**, **`NaN`**, **`0`**, the empty string **`" "`**, and **`undefined`**. If condition is any of these, the result of the conditional expression will be **`exprIfFalse`**.

Ternary operator can also be chained just like multiple *`if/else`* statements.

{{< highlight jsm  "linenostart=1"  >}}
function ifElseChain() {
  let age = 12;
  if(age >= 0 && age <= 12 ){
    return "You are Pre-teen"
  }
  else if (age >= 12 && age <= 18){
    return  "You are a Teenager"
  }
  else {
    return "You are grown ass adult"
  }
}

//equivalent ternary operator
function ternaryChain(){
  return (age >= 0 && age <= 12 ) ? "You are Pre-teen"
         :(age >= 12 && age <= 18) ? "You are a Teenager"
         :"You are grown ass adult"

}
{{< /highlight >}}

Ternary operators are usually used for conditional rendering in React. Even for other case and every javascript code, the principle is same and quite straigtforward.

Let's look at a simple example from a functional React component. This simply determines what to render depending on what the current value of `isLoading` is.

{{< highlight jsx  "linenostart=1"  >}}
function LoadingIndicator({ isLoading }) {
  return (
    <div>
      { isLoading            // checks the value of isLoading
        ? <p>Loading...</p>  // if true, render "loading..."
        : null               // if false, render nothing as in null
      }
    </div>
  );
}
{{</ highlight >}}
### Destructuring assignments

> The **destructuring assignment** syntax is a JavaScript expression that makes it possible to unpack values from `arrays`, or *`properties`* from `objects`, into **distinct variables**.

*`Destructuring`* is one of the most common concept you'd find looking at any React Code base. It is quite straightforward to decipher but can look daunting for beginners at first.
We'll look at this in two parts, **array** and **object** destructuring in progressively complex example.

* **Array destructuring**

> Let's say you have an array of of my profile.
{{< highlight jsm  "linenostart=1"  >}}
// with ES5
const profile = ["Ola", "John", "ElCapito", "SWE", "Nigeria"]
let firstName =  profile[0]
let lastName = profile[1]
let nickName = profile[2]

// With ES6 destructuring

let [firstName,lastName,nickName] = profile
console.log(firstName,lastName,nickName) //Ola John ElCapito
{{</ highlight >}}
It is obviosuly neater with the ES6 destructuring.

* **Skipping items**

One thing to take note of, is that the destructured variables has to be mapped to the same index as the destructured array. If you want to access let's say the 4th element in the array at **`index 3`("Nigeria")** and map it to variable **country**, you can't just simply do `let [country] = profile`. You will need to take account of the first 3 elements. What if you don't need them? Then you need a comma as a placeholder to skip items in the array you do not need. Let's have a look

{{< highlight jsm  "linenostart=1"  >}}
const profile = ["Ola", "John", "ElCapito", "SWE", "Nigeria"]
let [firstName,lastName, , job,country] = profile
console.log(firstName,lastName,job,country) //Ola John SWE Nigeria

/* Notice that I have replaced the nickname variable with a comma
   You can skip as much element as you need by replacing them with a coma */

// let's skip firstName and job
let [, lastName,nickName, , country] = profile
console.log(lastName,nickName,country) //John ElCapito Nigeria
{{</ highlight >}}

* **Default values**

> A variable can be assigned a default, in the case that the value unpacked from the array is *`undefined`* Let's consider an example below.
{{< highlight jsm  "linenostart=1"  >}}
const profile = ["John", "ElCapito"]

let [realName, nickName, superPower] = profile
console.log(realName, nickName, superPower ="Javascript") // "John ElCapito undefined"
{{</ highlight >}}

We can use a defult value instead. So if the value should ever be *`undefined`* it is replaced by this *`deafult`* value.Like so:

{{< highlight jsm  "linenostart=1"  >}}
let [realName, nickName, superPower ="Javascript"] = profile
console.log(realName, nickName, superPower) // "John ElCapito Javascript"
{{</ highlight >}}

* **Swapping Variables**

ES6 array destructuring gives a more elegant way to swap variables in an array. With ES5 you had to rely on some cumbersome XOR tricks to get this done
{{< highlight jsm  "linenostart=1"  >}}
let realName ="John";
let nickName = "El Capito";    // this semicolon is mandatory.Otherwise you get an error.

[realName, nickName] = [nickName, realName]
console.log(realName) // "John"
console.log(nickName) // "ElCapito"
{{</ highlight >}}


* **Object destructuring :fire:**

Destructuring simply means breaking a more complex entity into smaller parts. *`Object destructuring`* helps breaks down a objects properties into smaller variables. This concept is very common in modern Javascript frameworks. *`Object destructuring`* seems to be much common that it's array counter part. Let's imagine you have an object containing data for a college student. It is much easier to work with a destructured version of the data when all variables have been broken down (**`destructured`**) into smaller parts. Let's take a stab at this with examples.
{{< highlight jsm  "linenostart=1"  >}}
const student = {
  name: "Ola John",
  faculty: "law",
  age: 25,
  expectedYearOfGraduation :  2052,
  isStudentBroke: true,
  gpa: 0.42
}

// Lets make sense of this student's data

//without destructuring
let name = student.name
let faculty = student.faculty
let age = student.age
let expectedYearOfGraduation = student.expectedYearOfGraduation

let gpa = student.gpa
let broke = student.broke

console.log(`My name is ${name}, I am in Faculty of ${faculty}.
I am ${age}years old. My GPA is ${gpa} and ${isStudentBroke} I am really broke`)
Output: // My name is Ola John, I am in Faculty of law.
        //I am 25years old. My GPA is 0.42 and true I am really broke
{{</ highlight >}}

You can easily see how repitive the code is, how many time I had to assign variables from the `student` variable with `let`. Asides the verbosity and repition, this is prone to error. I could easily misspell one of the variables after repeatedly declaring over and over. What if we could just extract all this variables with one variable assignment? Easy-peasy with *`Object destructuring`*. All you have to do is reference the variables we want to destructure into with the key in the object we are extracting from. "Enough jargons...show me the code" I heard you!!!

> Here we go. Let's destructure the previous *`student`* example. .
{{< highlight jsm  "linenostart=1"  >}}
const student = {
  name: "Ola John",
  faculty: "law",
  age: 25,
  expectedYearOfGraduation :  2052,
  isStudentBroke: true,
  gpa: 0.42
}
let { name, faculty, age, expectedYearOfGraduation, gpa, broke } = student

console.log(`My name is ${name}, I am in Faculty of ${faculty}.
I am ${age}years old. My GPA is ${gpa} and ${broke} I am really broke`)
// My name is Ola John, I am in Faculty of law.
//I am 25years old. My GPA is 0.42 and true I am really broke
{{</ highlight >}}
Isn't that beautiful?, comapred to what we had up there. Terse, straight to the point and elegant.

* **Assigning to new variable names**

> A property can be unpacked from an object and assigned to a variable with a different name than the object property

What does that even mean?. It implies that you can decide to give a different name other than the name in the original object. Let's go back to our broke student. Imagine for a second that we have to use the *`isStudentBroke`* and/or the *`expectedYearOfGraduation`*  properties from the **student** object, we can rename them to something shorter for easy reference throughout the code. You could rename for many other reasons, but how?. Code sample to the rescue once more.
{{< highlight jsm  "linenostart=1"  >}}
let { name, faculty, age, expectedYearOfGraduation: year, gpa, isStudentBroke: broke} = student
// now expectedYearOfGraduation is now re-assgned to  year and isStudentBroke to broke
{{</ highlight >}}

* **Assigning to new default values**
> A variable can be assigned a default, in the case that the value unpacked from the object is *`undefined`*.

Let's say our broke student decides to not enter his broke status or gpa for instance. If we try to destructure them , we get undefined. Let's see:
{{< highlight jsm  "linenostart=1"  >}}
const student = {
  name: "Ola John",
  faculty: "law",
  age: 25,
  expectedYearOfGraduation :  2052,
}
let { name, faculty, age, expectedYearOfGraduation, gpa, isStudentBroke } = student

console.log(name, faculty, age, expectedYearOfGraduation, gpa, isStudentBroke ) //Ola John law 25 2052 undefined undefined
{{</ highlight >}}

What if we wanted to have default values whenever *`broke`* students with low *`gpa`* decides not to enter these details. We can set default values just like with *`array destructuring`*
{{< highlight jsm  "linenostart=1"  >}}
const brokeStudent = {
  name: "Ola John",
  faculty: "law",
  age: 25,
  expectedYearOfGraduation: 2052,
}
let { name, faculty, age, expectedYearOfGraduation, gpa = 0, broke = true } = brokeStudent

console.log(name, faculty, age, expectedYearOfGraduation, gpa, broke) //Ola John law 25 2052 0 true
{{</ highlight >}}

* **Assigning to new variables names and providing default values**

> We could also assign new variable names and provide default values all within one destructuring assignment. Like so:
{{< highlight jsm  "linenostart=1"  >}}
let { name, faculty, age, expectedYearOfGraduation: year = 2025 , gpa, isStudentBroke:broke = true } = student
{{</ highlight >}}


> This is the end of Part(1) of this article.






For more in depth documentation of *`Object destructuring`* I highly reocmmend the [mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) documentation.

Stay tuned for the second and concluding part of these series. We'll be covering the underlisted topics in more details.

* [High-order Functions](#high-order-functions)
* [Arrow functions](#arrow-functions)
* [Map, Filter and Reduce](#map-and-filter)
* [Spread/Rest Operator](#spread-rest-operator)
* [Default function parameters](#default-function-parameters)
* [ES6 Classes](#es6-classes)
* [Async JavaScript](#async-javascript)
