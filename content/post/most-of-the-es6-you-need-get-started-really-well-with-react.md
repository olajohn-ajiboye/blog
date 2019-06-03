---
title: "The 80/20 rule - Most of the ES6 You Need to know to have a better start with React."
date: 2019-06-02T23:27:34+03:00
tags: ["React", "Javascript","ES6","Coding"]
draft: false
---

In an ideal world,you'd probably be required to learn a ton of Javascript before you take a dive into React or any Javascript Framework for that matter.Unfortunately, the world isn't ideal. However I believe getting the Fundamentals of the Javascript you'd be using 80% of the time in React will make your journey less excrutiating.At least, this is what I found out. Let's try to look at those ES6 feature you are most certainly going to come across majority of the time.



We'll be taking a dive at the underlisted ES6 concepts.Some are more simpler that the other and you'll probably know them already if you have worked with Javascript for as little as few weeks. But I guess it's fine to go over them nonetheless. Let the part begin!

* [Varibale declaration with let/const](#variable-declaration-with-let-const)
* [Template literals (Template strings)](#template-literals-template-strings)
* [Ternary Opearors ..that weird one with &&](#ternary-opearors)
* [ES6 Module system](#eS6-module-system)
* [Destructuring assignments](#destructuring-assignments)
* [High-order Functions](#high-order-functions)
* [Arrow functions](#arrow-functions)
* [Map and Filter](#map-and-filter)
* [Spread/Rest Operator](#spread-rest-operator)
* [Default function parameters](#default-function-parameters)
* [ES6 Classes](#es6-classes)




### Variable declaration with let/const
Before Es6 ``var`` was the defacto keyword for declaring variables. With ES6 two new keywords ```let``` and ```const``` were introduced for variable declartions. But what are the differences and why bother with them anyways?.
There are couple of gotchas with decalring variables with the ```var``` keyword. The ```scope``` and ```hositing``` were cheif amongst the issue with ```var```. But what does these 2 even mean.Let's take a look.

The scope of ```var``` is the entire function block in which it is defined.It is also available globally when declared outside a function. `let` on the other hand is limited to the block scope. What does that even mean?, let's take a look at an example.

**Global Object**

{{< highlight jsm "linenostart=1" >}}
var testOfVar = "I am 'Var'";
let testOfLet = "I am 'Let'"

console.log(this.testOfVar) // OutPuts I am 'var'
console.log(this.testOfLet) // OutPuts undefined
{{< / highlight >}}
As you would see the variable declared with `let` is not attached to the global object and therefore not accesible globally.On the other hand `var` decalred variable is accessible globally and therefore can be accidentally altered anywhere within the program.This is really bad and can be cause of debugging frustrations.It is therefore recommended to use `let` instead of `var` always.

**Scope**

The `let` statement declares a block scope local variable, optionally initializing it to a value.
**let** allows the variable declaration to be limited in scope.It is limited to the scope block or statement, or expression on which it is used as well as any sub-blocks. Unlike `var` where the variable is hoisted to the top of the function scope and it is accessible through the entire function scope regardless.

Let's look at this in codes to solidify these concepts.
{{< highlight jsm "linenostart=1" >}}
function testOfVar() {
  var myName = "John" // this will later be modified within the if block.
  if (true) {
    var myName = "Joshua"
    console.log(myName) // this is same variable as the first 'myName'
                        // scope is ignored.Modifies the variable declared ontop of function.
                        // Outputs 'Joshua'
  }
  console.log(myName) // myName has been modified within the if block.
                     // Every reference of myName within this function block now reffers to "Joshua"
}

function testOfLet() {
  let myName = "John" // this will not be modified
  if (true) {
    let myName = "Joshua"
    console.log(myName) // this is another variable. The scope is limited to this if block
                        // scope is respected, this variable will not modify anyting outside this block
                        // Outputs 'Joshua'
  }
  console.log(myName) /* myName declared at the top of the function is not modified
                      within the if block */
                      // Output 'John'
}

{{< / highlight >}}


**Re-Declaration**

With `var` it was possible to re-declare a variable which isn't a good thing.You could easily re-declare a variable that has already been decalred to reference something else initially. It will introduce a bug without you noticing. `Let` throws an error when you try to re-declare a varibale within same scope.

{{< highlight jsm "linenostart=1" >}}
function testOfVar() {
  var myName = "John";
  var myName = "Joshua" // this doesn't make sense, but var makes it possible.
  console.log(myName)  // Outputs 'Joshua'
}

function testOfLet() {
  let myName = "John";
  let myName = "Joshua" // this doesn't make sense, but var makes it possible.
  console.log(myName) // SyntaxError 'myName' is already been declared
}
{{< / highlight >}}

In summary avoid using the `var ` statement. You should almost always pick the ``let`` keyword when you need to declare a variable.

**What about `const`?**

`const` is quite similar to ``let``. The major difference is that ``const`` can not be re-assigned.Do not confuse re-assigning with re-declaring. In most cases you'd want to declare a variable and make some re-assignments depending on certain conditions. Let's look at an example.
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



### Template literals (Template strings)

Template strings simplifies the way we work with strings. String interpolation and multi-line strings are probably most of template strings you'll encounter working with modern Javascript.Template strings are declared using the back ticks.
{{< highlight jsm "linenostart=1" >}}
//ES5
let exampleString = "I am String" // or  exampleString = 'I am a String'

//ES6
let exampleString = `I am String`
{{< /highlight >}}
Template strings are easier explained with example code.

**Multi-line strings**

You can get strings in multiple lines by usinhg the string literals instead of the more cumbersome ES5 alternative.
{{< highlight jsm "linenostart=1" >}}
//ES5
console.log('string text line 1\n' +
'string text line 2');

//ES6
console.log(`string text line
string text line 2`)
{{< /highlight >}}
{{< highlight jsm "linenostart=1" >}}
A react JSX example
//ES5
console.log('string text line 1\n' +
'string text line 2');

//ES6
console.log(`string text line
string text line 2`)
{{< /highlight >}}

**String Interpolation**

Template literals provide an easy way to interpolate variables and expressions into strings.
You do so by using the ${...} syntax:

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
### Ternary Opearors ..that weird one with &&
### ES6 Module system
### Destructuring assignments
### High-order Functions
### Arrow functions
### Map and Filter
### Spread/Rest Operator
### Default function parameters
### ES6 Classes
