---
title: "Part 3 - ES6 Template Literals"
author: "Author Name"
tags: ["Strings", "ES6", "Coding", "Javascript"]
date: 2019-06-06T21:58:53+03:00
draft: false
---

### Template literals (Template strings) :muscle:

String interpolation and multi-line strings are probably most of the template strings concepts you'll encounter working with modern Javascript.Template strings are declared using backticks.

<!--more-->



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
 const introduction = "My name is " + name + " I am a " + age + "year old " + country
 console.log(introduction)
}

//ES6
function es6String() {
 const name = `John`
 let age = 20
 const country = `Nigerian`
 const introduction = `My name is ${name} I am a ${age}old ${country}. I will be ${age + 5} years old in 5 years`
 console.log(introduction)
}
{{< /highlight >}}

> *Template strings are commonly used with styled-components library in React*
{{< highlight jsm "linenostart=1" >}}
//Styled-Component example
const fontSize = calculateFontSize();
RedCenteredText = styled.h1`
  color: red;
  text-align: 'center;
  font-size: ${fontSize}`
{{< /highlight >}}



