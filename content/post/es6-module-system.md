---
title: "Part 4 - ES6 Module System"
author: "Ola John"
tags: ["ES6", "beginner", "coding"]
date: 2019-06-06T21:59:11+03:00
draft: false
---

ES6 modules are one of the more critically important concept you will come across in every React program. I will not delve in-depth as to why the module patterns is important. You can read more about that [here](https://exploringjs.com/es6/ch_modules.html), [here](https://www.freecodecamp.org/news/how-to-use-es6-modules-and-why-theyre-important-a9b20b480773/) and [here](https://www.sitepoint.com/understanding-es6-modules/). Instead I will provide a quick overview of what they are and how to quickly get started with them.

<!--more-->

### ES6 Module system :smiley:

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
