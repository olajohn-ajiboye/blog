---
title: "The 80/20 rule - Most of the ES6 You Need to know to have a better start with React."
date: 2019-06-02T23:27:34+03:00
cover: "/img/react-es6.jpg"
tags: ["React", "Javascript","ES6","Coding"]
draft: true
---

If you have worked with a few Javascrit frameworks, one of the more perplexing realisation is that you have no idea where the frameworks' margic stops and where Javascript starts. I have worked a little bit with Angular and mostly with Vue. With Angular written in Typescript, it was confusing for me as a newbie what part of the code I was looking at was simple Typescript or some Angular abracadabra. Vue was a liitle bit different, you could easily indentify which part was plain Javascript or which was Vue's magic.You just needed to understand few concepts and look ot for everywhere you see "v-".


Here comes React. It looked like a breath of fresh air because I could see it had very little API to figure out and most thing looked like vanilla JS. But wait, what!...I still didn't understand some of the strange looking syntaxes. Even though I could bet this wasn't some React Magic, it still didn't make sense. What is it with these weird looking varibale assignments.Oh God!, why the heck am I seeing ```bind(this)``` everywhere. Ok, I thought I understood how arrow functions functioned (lols), turns out I didn't a lot about its quirks. Since I wasn't planning on giving up , I slowly figured out some of the concepts. After dozens of articles and tutorials a whole lot of the syntax and concepts began to make sense. This post article is an attempt to piece together all I have learnt overtime, and how I wish I had learnt them in a deiberate step-wise manner before diving into React.

So Here we go:

The ES6 concepts we'll be looking at are as follows.Some are more simpler that the other and you'll probably know them already if you have worked with Javascript for as little as few months. But I guess it's fine to go over themm nonetheless.

* [Varibale declaration with let/const](#variable-declaration-with-let-const)
* [Object literal Shorthand](###object-literal-shorthand)
* [Ternary Opearors ..that weird one with &&](##ternary-opearors)
* [ES6 Module system](###eS6-module-system)
* [Destructuring assignments](###destructuring-assignments)
* [High-order Functions](###high-order-functions)
* [Arrow functions](###arrow-functions)
* [Map and Filter](###map-and-filter)
* [Spread/Rest Operator](###spread-rest-operator)
* [Default function parameters](###default-function-parameters)
* [ES6 Classes](###es6-classes)




### Variable declaration with let/const
Before Es6 var was the defacto keyword for declaring varibales. With ES6 two new keywords ```let``` and ```const``` were introduced for variable declartions. But what are the differences and why bother with them anyways. Let's take a dive into each one of them.
#### Var
With ES5 var was the deafult for varibale declarations. However, there are couple of gotchas with decalring variables with the ```var``` keyword. The ```scope``` and ```hositing``` were cheif amongst the issue with ```var```. But what does these 2 even mean.Let's take a look.

The scope of ```var``` is the entire function block. Lets examine this with an example

<!-- ```javascript
var myName = "John"
function testVar() {
var myName = "Now Joshua"
console.log(myName)
}
console.log(myName)


``` -->


### Object literal Shorthand
### Ternary Opearors ..that weird one with &&
### ES6 Module system
### Destructuring assignments
### High-order Functions
### Arrow functions
### Map and Filter
### Spread/Rest Operator
### Default function parameters
### ES6 Classes
