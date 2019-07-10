---
title: "Learn React Hooks and Context API by Building a Recipe Search App Part 1"
author: "Ola JOHN"
tags: ["React", "Hooks","ES6","Beginner"]
date: 2019-06-27T21:37:30+03:00
draft: false
---


This is a beginner tutorial in understanding `React Hooks`  and the `Context API`. In reality, this is more of an attempt to explain these concepts to my self and to interested readers.


This will be in two parts with this first part focusing on the basics of `hooks`. The other part will hopefully focus more on more advanced use cases and the `Context API`. We'll implement the search functionality and convert to using the  `Context API` for managing states and avoiding `props drilling`

## How are we going to learn?

We'll look at some contrast on handling the same state logic with a class component, and then with `React Hooks`.  We will be building a Recipe Search application to solidify the concepts, we'll then have a bottle of red wine to celebrate😃. I believe messing with the code is the best way to learn.

# Final Version of what we'll be building

We'll build a simple clone of the food2fork website. This will not be a fully fledged version of it, but we'll use their API with the top 30 recipes. Add some UI logic of our own just to mess with the states and see how we handle them with `hooks`.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561757356255/kgOo1cmfl.gif" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:320px; width:520px" >



## Prerequisite
- Basic Knowledge of React
- Javascript ES6 fundamentals [Array methods, Destructuring, etc]



## So what is a React Hook?

First things first, so what is React Hook? From [documentation](https://reactjs.org/docs/hooks-state.html)

> **What is a Hook?** A Hook is a special function that lets you **“hook”**  into React features. For example, useState is a Hook that lets you add React state to function components.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561758534907/eg-bho7_D.jpeg" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:320px; width:520px" >
</div>


In simple English, hooks allow us to "hook" into specific React Features. For example `useState` as the name implies helps us to use state feature in React where we otherwise wouldn't be able to use it, i.e inside a function component. We'll explain a little bit in details the syntax usage etc through building the recipe search application.

## Set Up

We'll be using create-react-app to bootstrap the application. I have created a repo with a barebone create react app that we can quickly get started with. Simply clone to follow along. Run the command below:

> git clone https://github.com/olajohn-ajiboye/Blog-React-Hook-Tutorial.git
> cd Blog-React-Hook-Tutorial

Going back to the app we'll be building. We shall be using the food2fork API to fetch and search a list of 30 top recipes. However, the API has a limited query we can make per day. I have created the exact JSON response from them for this tutorial. This will be served from here [here](https://api.myjson.com/bins/t7szj) so that we won't be hitting their server too frequently.

Let's create the component folder in our `src` folder to hold the different components our app is likely to have. Take a moment to think about that, there will be a component displaying each `Recipe`, a `RecipeList `component to render a list of the recipes, a `RecipeSearch` component and `RecipeDetails` component for displaying additional details for each `Recipe`. All components will be functional components, since using hook is the point of this tutorial.

If you did everything correctly, you should have a folder structure like below. You can also clone the `second` branch in the repo  [here]() to jump to this point in the tutorial.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561582276567/x5RzZLgNz.png" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:320px; width:520px" >
</div>


If you have not already, one extension I will recommend installing is the `ES7 React/Redux/GraphQL/React-Native snippets`. It speeds up your React development by allowing you to type shorthands to get some React Snippet. You can find out more [here](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

## So let's get `Hooked`
Why even Hooks?

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561588938878/yhBMIOEyV.gif" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:320px; width:520px" >

</div>



One of the most common things you will do in any frontend application is fetching and displaying data, and manipulating UI based on UX. React is no different. One of the common use cases of **state** is to store data from an API call.  Before `hooks`, if you needed to have a `state` of any type within your app, you absolutely have to use a class component. You'd also need to fetch data asynchronously within the `componentDidMount` lifecycle. For many, this isn't a mighty problem, but the React team believes it gives rise to the tight coupling of component logic. Also in more complex apps, it is hard to reuse stateful logic. Don't take my word for that, just read the motivation for hooks [here](https://reactjs.org/docs/hooks-intro.html)

Let's look at how we'll handle fetching data from the REST API I have created in a classical component, before moving to how we are going to handle it with `hooks`



{{< highlight jsm  "linenostart=1"  >}}

import React, { Component } from 'react'
import RecipeList from './components/RecipeList

export default class test extends Component {
  constructor(props) {
    super(props)
    this.state = {
      apiResponse: [],
    }
  }
  componentDidMount() {
    fetch(`https://api.myjson.com/bins/t7szj`)
      .then(data => data.json)
      .then(apiResponse => this.setState({ apiResponse }))
  }
  render() {
    return (
      <div>
          <RecipeList recipes={this.state.recipes}>
      </div>
    )
  }
}
{{</ highlight >}}

> Let's take a look at the same code with Hook and Effect followed by an explanation


{{< highlight jsm  "linenostart=1"  >}}

import React, { useState, useEffect } from 'react';
import RecipeList from './components/RecipeList

function App() {
  const url = useState(`https://api.myjson.com/bins/t7szj`)
  const [recipes, setRecipes] = useState([])

  const fetchRecipe = async () => {
    const recipeData = await fetch(url)
    const { recipes } = await recipeData.json()
    setRecipes(recipes)
  }

  useEffect(() => {
    fetchRecipe()
  })

  return (
    <div className="App">
      <RecipeList recipes={recipes}>
    </div>
  );
}

export default App;

{{</ highlight   >}}
There are a couple of things that are obvious right away, we have imported useState and useEffect from `react`. These are the APIs exposed to us that enables us to use `React Hooks`. The `useState` Hook accepts the initial state. In the example above we have initialized it into an empty array. We expect to populate the array with data from the API call. This is equivalent to below in our class component.

{{< highlight jsm  "linenostart=1"  >}}
 this.state = {
      apiResponse: [],
    }
{{</ highlight >}}

Furthermore, the `useState` returns a pair of values to us. They are the current state and a function that updates it. So we get back `[currentState, setStateFunction]` from the use state. That is why we have written  ` const [recipes, setRecipes] = useState([])` in our application. Where `recipes` is an array to hold our recipe data. `setRecipe` is the function that enables us to update the state, this is equivalent to `this.setState` in a class component.

In case the syntax looks confusing, these are not **React** specific syntax, but plain ES6 Javascript. This is known as destructuring. Since the `useState` returns a pair of value, we have destructured it into an array. The name we choose for them does not affect their behavior, it is simply good practice to name them as `[name of your state, set+name of state]`, hence we have :


{{< highlight jsm  "linenostart=1"  >}}
const [recipes, setRecipes] = useState([])
{{</ highlight >}}

 If you need some refresher or primer on destructuring, I have written a little bit about it [here](https://olajohn.me/post/es6-destructuring/).

Another Javascript feature we need to take note of to fully understand what is going on here is `closures`. Due to `Javascript closure`, we have access to the destructured variables from the `useState` anywhere within our function. Hence, inside the `fecthRecipe`  function and anywhere within the `component` we can use the `setRecipe`  or any other variables without having to call it `this.setRecipe` etc.

> For the purpose of this tutorial. A simplified definition of `closures` is that it gives us access to the outer (enclosing) function's variables—scope chain, and also to their return values.

You can read more [here](https://www.codingame.com/playgrounds/6516/closures-in-javascript-for-beginners), [here](https://javascript.info/closure) and watch [here](https://www.youtube.com/watch?v=71AtaJpJHw0)

Let's quickly pass the props down our components and get them set up to display the list of Recipe. Since this isn't a `Hook` specific feature. I am going to skip it. You can find the updated repo up to this point here. I have also added styling to speed things up. The updated repo is on the ` third/hook-in-app.js` branch in the [repo](https://github.com/olajohn-ajiboye/Blog-React-Hook-Tutorial).


## Using the `useState`
At this point, your `App.js`  should look like below, we have simply passed down the recipes array inside our state as a `recipes props` to the `RecipeList` component. Note, I have also added a loading state with the `useState` and set it back to `false` after data is completely fetched. This the first example of using multiple states.


{{< highlight jsm  "linenostart=1"  >}}

import React, { useState, useEffect } from 'react';
import RecipeList from './components/RecipeList'
import RecipeDetails from './components/RecipeDetails'


function App() {
  const url = `https://api.myjson.com/bins/t7szj`
  const [recipes, setRecipes] = useState([])
  const [loading, setLoading] = useState(true)
  const fetchRecipe = async () => {
    const recipeData = await fetch(url)
    const { recipes } = await recipeData.json()
    setRecipes(recipes)
    setLoading(false)
  }
  useEffect(() => {
    fetchRecipe()
  })
  return (
    <div>
      {loading ? <h1 className="text-center">...loading</h1> : <RecipeList recipes={recipes} />}
      <RecipeDetails></RecipeDetails>
    </div>
  );
}

export default App;

{{</ highlight  >}}
Next, let's move to the `RecipeList` component and see what we have.

Here we have simply received the `recipes` passed as a `prop` from the parent `App` component and immediately `destructured` it --see line **5**. We then mapped over it, pass down each recipe to the `Recipe` component as `prop`. Nothing particularly interesting here.


{{< highlight jsm  "linenostart=1"  >}}
import React from 'react'
import Recipe from './Recipe'
import RecipeSearch from './RecipeSearch'

export default function RecipeList({ recipes }) {

  return (
    <>
      <RecipeSearch></RecipeSearch>
      <div className="container my-5">
        <div className="row">
          <div className="col-10-mx-auto  col-md-6 text-center text-uppercase mb-3">
            <h1 className="text-slaned text-center">Recipe List</h1>
          </div>
        </div>
        <div className="row">
          {recipes.map(recipe => {
            return <Recipe key={recipe.recipe_id} recipe={recipe} />
          })}
        </div>
      </div>
    </>
  )
}


{{</ highlight >}}

## Now to the fun part.
Inside the `Recipe` component, I have added some contrived `states` that are simple enough to fully understand. We'll make an attempt to go line by line, trying to understand what we are doing and how we are handling stateful logic with the `useState hook` . You should have the below in your `Recipe` component.

{{< highlight jsm  "linenostart=1"  >}}
import React, { useState } from 'react'

export default function Recipe({ recipe }) {
  const { image_url, publisher, title, recipe_id } = recipe
  const [showInfo, setShowInfo] = useState(false)
  const [recipeDetails, setRecipeDetails] = useState([])
  const { ingredients, social_rank } = recipeDetails

  const handleShowInfo = async (e) => {
    const { id } = e.target.dataset
    const response = await fetch(`https://www.food2fork.com/api/get?key=7cdab426afc366070dab735500555521&rId=${id}`)
    const { recipe } = await response.json()
    setRecipeDetails(recipe)
    setShowInfo(!showInfo)
  }
  return (
    <>
      <div className="col-10 mx-auto col-md-6 col-lg-4 my-3">
        <div className="card">
          <img src={image_url} alt="recipe" className="img-card-top" style={{ height: "14rem" }} />
          <div className="card-body text-capitalize">
            <h6>{title}</h6>
            <h6 className="text-warning">
              Provided by: {publisher}
            </h6>
          </div>
          <div className="card-footer">
            <button type="button" style={{ margin: `13px` }} className="btn btn-primary text-center" data-id={recipe_id} onClick={handleShowInfo}>More Info</button>
            {showInfo &&
              <button key={recipe_id} type="button" style={{ margin: `13px` }} className="btn btn-success text-center font-weight-bold" >{social_rank}</button>}
            {showInfo ?
              ingredients.map((i, index) => {
                return <ul key={index} className="list-group">
                  <li className="list-group-item" >{i}</li>
                </ul>
              })
              : null}
          </div>
        </div>
      </div>
    </>
  )
}
{{</ highlight >}}
Let's understand the code above. Like previously, we have received the `recipe` as a `prop` from it's parent component which is the `RecipeList` we have then destructured it immediately within the function argument. We then went ahead to further destructure the parts we intend to use from the `recipe` object. I know what the `recipe` object contains because I have tested the API, so this isn's some magic. This is equivalent to below

{{< highlight jsm  "linenostart=1"  >}}
export default function Recipe(props) {
    const recipe = this.props.recipe
    const { image_url, publisher, title, recipe_id } = recipe

}

{{</ highlight  >}}

Now to some useful state. If you checked the application. You'd realize that when  we clicked on the **more** details button we get the extra information related to that recipe, specifically the `list of ingredient` and the `social rating`. Take a peek at the gif on top for a refresher. So we need some kind of state to handle the desired user interaction.

Think about it for a moment, we need a way to toggle between if more info should be shown or not. We also need a way to fetch the info for the specific recipe. The desired result will then be stored in some kind of state. Viola, we have identified at least 2 states. Hence in our Application, we have a `showInfo` and a `recipeDetails` state.

With the information we have, let's use the `useState Hook` to work with this.

- First, we declare the `showInfo` state and the function to set the showInfo `setShowInfo` (equivalent to `this.SetState`). We set the value to `false`
- Second, we declare the `recipeDetails` and the `setRecipeDetails`. We set the value to an empty array `[]`.

That's hopefully simple enough, we have set up the initial states. And prepared for handling state change with `setShowInfo` and  `setRecipeDetails`.

Moving to the `handleShowInfo` function. This is an `async` function that basically helps to fetch data. And also handling state change to show the info or not. Let's take it apart line by line.
Since we intend to call  `handleShowInfo` on click of a button we have access to the event object. Inside the button, we have set the `recipe_id` as a `data-attribute`. This enables us to grab the `id` for the specific recipe. Inside the `handleShowInfo,` we then get hold of the `id` by extracting it out of the `event.target` property. Since we need to get additional info, there is a need to make an `HTTP` request with the `id`. This is what we have done next and awaited the response. We then converted the value to a `json` and store the value in ``const recipe``.

> **Note: ** You may need to get an API Key from [food2fork](https://www.food2fork.com/). The current key will probably be out of limit.

The `recipe` we get as response is the slotted inside the `setRecipeDetails` to serve as and updater for the `recipeDetails`. At this point, we have simply set the state of `recipeDetails` to the array response variable `recipe`. This is equivalent to


{{< highlight jsm    >}}
this.setState{
recipedDetails: recipe
}
{{</ highlight  >}}
Another thing is that we set the value of `showInfo` to the opposite of what it was. This is what creates the toggle effect whenever the button is clicked. This is equivalent to.

{{< highlight jsm  "linenostart=1"  >}}
this.setState{
showInfo: !showInfo
}

{{</ highlight  >}}
And that's about it, in the returned `jsx` we conditionally rendered the info based on the state of `showInfo` when the button is clicked. We have additionally mapped over the array of ingredients to display them as the additional info.

This is the end of this very basic introduction of `hooks` and perhaps an oversimplification. In the next iteration of this series, we shall examine hooks in more details and then learn the ` Context API`.

Hope you enjoyed the introduction. Feedbacks are welcomes. Stay tuned for the next update and see you soon. Thank you!

<img src="https://cdn.hashnode.com/res/hashnode/image/upload/v1561763819140/b8txZnUyU.jpeg" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:300px; width:320px"  alt="thank-you-meme-03.jpg">
