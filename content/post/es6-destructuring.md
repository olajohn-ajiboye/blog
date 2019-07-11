---
title: "Part 5 - ES6 Destructuring"
author: "Ola John Ajiboye"
tags: ["Destructuring", "ES6", "Beginner","Javascript"]
date: 2019-06-06T21:59:33+03:00
draft: false
---
### Destructuring assignments :ok_hand:

> The **destructuring assignment** syntax is a JavaScript expression that makes it possible to unpack values from `arrays`, or *`properties`* from `objects`, into **distinct variables**.


<!--more-->



*`Destructuring`* is one of the most common concept you'd find looking at any React Code base. It is quite straightforward to decipher but can look daunting for beginners at first.
We'll look at this in two parts, **array** and **object** destructuring in progressively complex example.

* **Array destructuring :boom:**

> Let's say you have an array of my profile.
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

One thing to take note of, is that the destructured variables have to be mapped to the same index as the destructured array. If you want to access let's say the 4th element in the array at **`index 3`("Nigeria")** and map it to variable **country**, you can't just simply do `let [country] = profile`. You will need to take account of the first 3 elements. What if you don't need them? Then you need a comma as a placeholder to skip items in the array you do not need. Let's have a look

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

> A variable can be assigned without a value, in that case, the value unpacked from the array is *`undefined`* Let's consider an example below.
{{< highlight jsm  "linenostart=1"  >}}
const profile = ["John", "ElCapito"]

let [realName, nickName, superPower] = profile
console.log(realName, nickName, superPower ) // "John ElCapito undefined"
{{</ highlight >}}

We can use a default value instead. So if the value should ever be *`undefined`* it is replaced by this *`default`* value. Like so:

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

Destructuring simply means breaking a more complex entity into smaller parts. *`Object destructuring`* helps breaks down a objects properties into smaller variables. This concept is very common in modern Javascript frameworks. *`Object destructuring`* seems to be much common that it's array counter part. Let's imagine you have an object containing data for a college student. It is much easier to work with a destructured version of the data when all variables have been broken down (**`destructured`**) into smaller parts. Let's take a stab at this in those examples.
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

You can easily see how repetitive the code is, how many time I had to assign variables from the `student` variable with `let`. Asides the verbosity and repetition, this is prone to error. I could easily misspell one of the variables after repeatedly declaring over and over. What if we could just extract all this variables with one variable assignment? Easy-peasy with *`Object destructuring`*. All you have to do is to reference the variables you want to destructure the key object, you are extracting from. "Enough jargons...show me the code" I hear you!!!

> Here we go. Let's destructure the previous *`student`* example.
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
I am ${age}years old. My GPA is ${gpa} and ${isStudentBroke} I am really broke`)
// My name is Ola John, I am in Faculty of law.
//I am 25years old. My GPA is 0.42 and true I am really broke
{{</ highlight >}}
Isn't that beautiful?, compared to what we had up there. Terse, straight to the point and elegant.

* **Assigning to new variable names**

> A property can be unpacked from an object and assigned to a variable with a different name than the object property.

What does that even mean? It implies that you can decide to give a different name other than the name in the original object. Let's go back to our broke student. Imagine for a second that we have to use the *`isStudentBroke`* and/or the *`expectedYearOfGraduation`*  properties from the **student** object, we can rename them to something shorter for easy reference throughout the code. You could rename for many other reasons, but how? Code sample to the rescue once more.
{{< highlight jsm  "linenostart=1"  >}}
let { name, faculty, age, expectedYearOfGraduation: year, gpa, isStudentBroke: broke} = student
// now expectedYearOfGraduation is now re-assgned to  year and isStudentBroke to broke
{{</ highlight >}}

* **Assigning to new default values**

> A variable can be assigned a default value, in the case that the value unpacked from the object is *`undefined`*.

Let's say our broke student decides to not enter his broke status or gpa for instance. If we try to destructure them , we get *`undefined`*. Let's see:
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

What if we wanted to have default values whenever *`broke`* students with low *`gpa`* decide not to enter these details. We can set default values just like with *`array destructuring`*
{{< highlight jsm  "linenostart=1"  >}}
const brokeStudent = {
  name: "Ola John",
  faculty: "law",
  age: 25,
  expectedYearOfGraduation: 2052,
}
let { name, faculty, age, expectedYearOfGraduation, gpa = 0, broke = true } = brokeStudent

console.log(name, faculty, age, expectedYearOfGraduation, gpa, broke) //Ola John law 25 2052 0 true
// now every broke student who didn't have those properties set will be considered broke and with gpa of 0...
{{</ highlight >}}

* **Assigning to new variables names and providing default values**

> We could also assign a new variable `name` and provide all default values within one destructuring assignment. Like so:
{{< highlight jsm  "linenostart=1"  >}}
let { name, faculty, age, expectedYearOfGraduation: year = 2025 , gpa, isStudentBroke:broke = true } = student
{{</ highlight >}}

For more in depth documentation of *`Object destructuring`* I highly recommend the [mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Object_destructuring) documentation.
