---
title: "Part 2 - ES6 Ternary Operators"
author: "Ola John Ajiboye"
tags: ["es6", "beginner", "coding"]
date: 2019-06-06T21:58:34+03:00
draft: false
---

### Ternary Operators :heart_eyes:

The conditional (ternary) operator is the only JavaScript operator that takes three operands. This operator is frequently used as a shortcut for the **if** statement.
The ternary operator as the word **"ternary"** implies has three main parts. The first is the **condition**, that must resolve to a boolean(truthy or falsy) values. The second part is the expression we'd like to resolve to if the `condition` is **true** and the third part is when the `condtion` is **false**.

<!--more-->

### Ternary Operators :heart_eyes:
The conditional (ternary) operator is the only JavaScript operator that takes three operands. This operator is frequently used as a shortcut for the **if** statement.
The ternary operator as the word **"ternary"** implies has three main parts. The first is the **condition**, that must resolve to a boolean(truthy or falsy) values. The second part is the expression we'd like to resolve to if the `condition` is **true** and the third part is when the `condtion` is **false**.

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
