---
title: "Learn React Hook and Context Api Part(2)"
author: "Ola John"
tags: ["React", "Beginner", "Javascript", React Hooks"]
date: 2019-07-04T13:45:08+03:00
draft: false
---

This is the second part of the series in learning **React Hooks** and the **Context API**. In the  [first part](https://dev.to/mongopark/let-s-learn-react-hooks-and-context-api-by-building-a-recipe-search-app-39pc) we learn the basics of `useState`, built the first parts of the Recipe Search application. The full code can be found on [github](https://github.com/olajohn-ajiboye/Blog-React-Hook-Tutorial)

<!--more-->



## Overview of what we'll learn in this section
In this section, we'll work a little bit more with `states` just to completely understand the concept. We'll explain `useEffect`. Build the `RecipeSearch` component. We'll then refactor our state management strategy to make use of the `Context API(useContext)`. Exciting right?

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561923790914/kbyoLT-ka.gif" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:320px; width:520px" >


## First things ...
We won't be making too many comparisons with classical components from here forward. Instead, we'll focus more on learning the fundamentals of `hooks`. From the last part, I have extracted the `RecipeDetails` into its own component. This is for two reasons
- It is the right thing to do, it's the point of component-based frameworks. It allows for code re-use.
- Also, this will give us ample opportunity to see the problem that arises with passing props around(`props drilling`). And how context API can help us.
More on that later! First, let's take  a stab at `useEffect`

# A quick primer of the `useEffect` hook

In the first part, we mentioned and used the `useEffect` hook but didn't give any detailed explanations. I am certain only a few of us realize the problem with the way we have utilized the useEffect hook in the first part.

> Hint: remember how we had to create our own separate JSON response. So as not to make too many API calls?.

Ha-ha,  we are doing something terribly wrong just using `useEffect` as we did before.  I will dive into that in a bit.

But first, what is this `useEffect` hook ?. As the name implies, it must have something to do with **effects** right?, if that's what you guessed you are absolutely correct. But what sort of effects?.

From official React  [documentation](https://reactjs.org/docs/hooks-effect.html), effects are actions related to data fetching, setting up a subscription, and manually changing the DOM in React components. These are all examples of side effects. Some call it "side effects" others just call it 'effects". We are referring to the same thing.If you’re familiar with React class lifecycle methods, you can think of `useEffect` Hook as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` combined.

## Using the `useEffect` hook correctly.

Let's relate this information to our application. In App.js we have fetched data from the food2fork API function and then called the function in the `useEffect` function. This is equivalent to calling it inside the `componentDidMount` function. Let's take a closer look.

But think about it for a second. What is the intended use of `componentDidMount`?, the answer is in the name!. You only ever want to run any function inside this function when the component is mounted. Let's go over that information slowly. **Mounting** - is phase the component is created (your code, and react’s internals) then inserted into the DOM.

But, if you have a function that causes a repeated effect inside a `componentDidMount` your component will continuously re-render except you find a way to clean it up. This is done usually within the `componentWillUnmount` lifecycle.

How then does `useEffect` manage to replace `componentDidMount` and `componentWillUnmount` within one function. Let's revisit our wrong use of the useEffect to have a better grab of this. When we call the `useEffect` like below, the components continuously re-renders because it has no clue how to *unmount*. This is a terrible performance bottleneck.

{{< highlight jsm  "linenostart=1"  >}}

// this isn't quite right. It works, but we'll see the problem later on
useEffect(() => {
    fetchRecipe()
  })
{{</ highlight  >}}
Let's do a post morterm to see the problem in action. Inside the ` fetchRecipe()` in the *App.js*, try console logging something to indicate when the component is rendered. We could have also just inspected the Network tab in the browser. But sometimes console logging and seeing it in action will probably drive the point home faster.

On the `third/hook-in-app.js` branch of the repo. Edit `App.js` add the console.log and pop open your console. You should have something similar to below in the **App.js** for this little experiment.

{{< highlight jsm  "linenostart=1"  >}}
import React, { useState, useEffect } from 'react';
import RecipeList from './components/RecipeList'
import RecipeDetails from './components/RecipeDetails'


function App() {
  const apiKey = `36920f6651c9cd9d91a6c3205cabaa19`
  let url = `https://www.food2fork.com/api/search?key=${apiKey}`
  const [showHomeButton, setShowHomeButton] = useState(false)
  const [recipes, setRecipes] = useState([])
  const [loading, setLoading] = useState(true)
  const [search, setSearch] = useState('')


  const fetchRecipe = async () => {
      const recipeData = await fetch(url)
      const { recipes } = await recipeData.json()
      setRecipes(recipes)
      setLoading(false)
      console.log("I am beign rendered again and again")
  }

  useEffect(() => {
    fetchRecipe()

  })

  return (
    <div>
      {loading ? <h1 className="text-center">...fetching {search} Recipe</h1> :
        <RecipeList recipes={recipes} />}
    </div>
  );
}
{{</ highlight >}}

You will get the horror below. What the hell is happening? This is not even acceptable behavior. We got away with it because of the **JSON** response I created. We could make unlimited `API` calls without issues. Obviously, this cannot be the right way to do things in production. So what's the problem? or even the solution.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561974360255/9GWqu9bta.gif" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:320px; width:520px" >



What is immediately clear is that the `fetchRecipe` continuously tries to fetch a response from the API and is being re-rendered every time by `useEffect`. Even though nothing has changed. We naturally would have had to clean this up in a class component by unsubscribing from the API call within another `componentWillUnmount`. Let's look at how `useEffect` handle this problem.

Now that we have appreciated the problem, let's think about the solution. Pause for a moment, what do we actually intend to achieve? In our case we want the `fetchRecipe` function to run only once when the component mounts - that is fetch data once and display it. Or when anything has an effect on the result of the function. At this point, nothing affects the result. So how do we then overcome this problem? Quite simple it turns out.

Inside the `useEffect` function, we simply pass an empty array as the second parameter. Hence, we are indicating to the `useEffect` not to update except when the component is first mounted since we expect nothing to cause fetch recipe function to update. Just like below.

{{< highlight jsm  "linenostart=1"  >}}
  useEffect(() => {
    fetchRecipe()
  }, []) // pass an empty array here.

{{</ highlight  >}}
And now if you went back into the `App.js` and do a console.log, you will realize `fetchRecipe` only runs once.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561977935130/TrKY4KO2_.gif" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:320px; width:520px" >



This is great, works well for our case. But what if we wanted to update and re-render after something causes  `fetchRecipe` to change. You might ask what could this be. Let's say we have a state that changes the *url* for fetching the data. It means the data from `fetchRecipe` depends on `url`. We just simply pass that parameter inside the array passed to `useEffect`. Hence, we can pass as many parameters as needed into the array.

{{< highlight jsm  "linenostart=1"  >}}

  useEffect(() => {
    fetchRecipe()
  }, [url,query])
{{</ highlight >}}

> What you are saying in human language to `useEffect` is that: only update when `url` or `query` parameter changes.
<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561978591770/BTRAroWZe.gif" style="display: block;margin-left: auto;margin-right: auto;width: 50%; height:320px; width:520px" >


I recommend  [RobinWieruch  ](https://www.robinwieruch.de/react-hooks-fetch-data/) tutorial for more examples if you still get confused.

Getting back to our app...

# Let's build the `RecipeSearch` component.

Armed with the knowledge of safely fetching data, we'll now switch to using the API key from food2fork.  Hence, we'll be doing some refactoring within `App.js`. We'll also introduce the best practice of having a `try catch` block inside the async function to catch any errors.

> clone the `4/feature/implemented-search` branch of the repo to follow along.

First, let's think of what we want to achieve with the search component and then think of how to achieve this. So, we want to enter a search,  when we click submit or press enter, we want to change the content of the page to the new search result. And we also would want to be able to go back to the top 30 recipes, and thus clearing the search (or return to the top 30 results, depending on how you think about it). See what I mean below.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1561987859280/PRfjvgOML.gif" style="display: block;margin-left: auto;margin-right: auto;width: 70%;  >


Now, we have a goal in mind, how do we write the code with `hooks`. Note, I am trying to make this as basic as possible, such that even with very basic React knowledge you can still build this. So not everything is exactly `hook` specific.

Immediately, it becomes obvious we definitely need some states to manage all those changes. First, we fetch another set of 30 **recipe** related to our search query. That's one state. We also have to decide when the `Go back Home` button should appear or disappear, you got it mate!. That's another state. Lastly, and perhaps the most important `state` is that holding our `search` parameter.

Like previously, let's look at how the code looks like in the `App.js` and then explain it line by line. Full code for search feature [here](https://github.com/olajohn-ajiboye/Blog-React-Hook-Tutorial/blob/feature/implemented-search/src/App.js)

{{< highlight jsm  "linenostart=1"  >}}
import React, { useState, useEffect } from 'react';
import RecipeList from './components/RecipeList';
import './App.css'


function App() {
  // remeber to replace key
  const apiKey = `36920f6651c9cd9d91a6c3205cabaa19`
  let url = `https://www.food2fork.com/api/search?key=${apiKey}`
  const [showHomeButton, setShowHomeButton] = useState(false)
  const [recipes, setRecipes] = useState([])
  const [loading, setLoading] = useState(true)
  const [search, setSearch] = useState('')
  const [error, setError] = useState('')

  const fetchRecipe = async () => {
    try {
      const recipeData = await fetch(url)
      const { recipes } = await recipeData.json()
      setRecipes(recipes)
      setLoading(false)

    } catch (e) {
      if (e) {
        setError(e.message)
        console.log(error)
      }
    }
  }

  const handleSubmit = async (e) => {
    e.preventDefault()
    try {
      setLoading(true)
      const searchUrl = `${url}&q=${search}`
      const searchedRecipeData = await fetch(searchUrl)
      const { recipes } = await searchedRecipeData.json()
      setRecipes(recipes)
      setLoading(false)
      setShowHomeButton(true)
    } catch (e) {
      console.log(e)
    }
  }

  const handleSearchChange = (e) => {
    setSearch(e.target.value)
  }

  const handleReturnHome = () => {
    fetchRecipe()
    setShowHomeButton(false)
  }

  useEffect(() => {
    fetchRecipe()

  }, [])

  return (
    <div>
      {loading ? <h1 className="text-center">...fetching {search} Recipe</h1> :
        <RecipeList
          search={search}
          handleSubmit={handleSubmit}
          handleSearchChange={handleSearchChange}
          recipes={recipes}
          showHomeButton={showHomeButton}
          handleReturnHome={handleReturnHome} />}
    </div>
  );
}

export default App;

{{</ highlight >}}

Noticed, how we have changed a couple of things. First, we are now safely making API requests directly. Hence, we are using our real API keys. It will become apparent why we can't continue using my `json` anyways. Let's go line, by line.

We have set a variable for the `url`. We know how the URL endpoint looks like from the [food2fork](https://www.food2fork.com/about/api) documentation. Next, we set up a state with for `showHomeButton` to handle visibility for the home button. And set the initial value to `false`. That is,
initially when we'll be in the home page, there's no point displaying home button. The next two `states` are already familiar. Then we have the `search` state,  and we set the original value to an empty string. Since we are not searching for anything yet when we first visit the home page.

Next, we have to manage how we respond to user typing in the search query. We have set up the `handleSearchChange` function to handle that. We have the `handleSubmit` button to take care of updating the data in the list to match the result from the search. And finally, we have the `handleReturnHome` function to obviously, you guessed right. Help us return home safely with just the 30 top recipes.

Let's examine how each function performs its duty.

First, `handleSearchChange` . The purpose is to capture user entry and then update the `search` state to equal user entry. This is quite straight forward as the function body is even just 1 line.

{{< highlight jsm  "linenostart=1"  >}}

 const handleSearchChange = (event) => {
    setSearch(event.target.value)
  }
{{</ highlight  >}}
Here is how it works, since we are responding to user input, we have access to the `onChange` property from the event object. We have simply grabbed the value from the `event.target.value` and updated the search state with this value using the `setSearch hook`. We then need to pass them as props all the way down to the `RecipeSearch` component like below. This is one of those things to solve with `Context API`.


Next, let's examine the most interesting function in  App.js. The `handleSubmit`. What does it do? Let's look at the code first and then explain.

{{< highlight jsm  "linenostart=1"  >}}
 const handleSubmit = async (e) => {
    e.preventDefault()
    try {
      setLoading(true)
      const searchUrl = `${url}&q=${search}`
      const searchedRecipeData = await fetch(searchUrl)
      const { recipes } = await searchedRecipeData.json()
      setRecipes(recipes)
      setLoading(false)
      setShowHomeButton(true)
    } catch (e) {
      console.log(e)
    }
  }
{{</ highlight >}}
Since this is going to be triggered on submit. We have access to the `event` object. First, we need to prevent the default behavior of `onSubmit` which is to reload the page, hence we have `   e.preventDefault()`. We set the loading state to true with  `setLoading(true)` indicating we are still fetching data. We then take the current state of `search` that is now equal to the input by the user. We use this to create a new API point based on the documentation we have from food2fork. And then fetches new data based on this search query. It then updates the current `recipe` state with `setRecipe` to match with the new data from the search query. Now that we have the data we set `loading` state to false using ` setLoading(false)`. Now the new recipe list is based on the search query, we need a way to go back home. Hence, we have set the  `showHomeButton` to true using `setShowHomeButton`.

Lastly, we have the `handleReturnHome` function whose job is to get us back home with the top 30 recipe list. So we simply fetched recipe as we did originally, this already sets back the `recipe` state back to how it was. We then set the `showHomeButton` back to false, making the button disappear when we are on the home page. We then simply passed down all the required states as props to child components where we'd use them.

## **`The Context API`**

React Context API is essentially a way to create global states that can be used anywhere within the application. This is an alternative to passing down props from grandparents to children and so on. It's been touted as a lighter alternative to Redux. In my experience, I will say it more closely resembles Vue's VueX. Where you have a single source of truth and can access that state globally.

Let's examine the case of props drilling in our application. You can imagine in an application simple as this, we already have to pass props from `App.js` through `RecipeList` and then to `RecipeSearch`. Even though RecipeList component does not need the `handlesSubmit` and `handlesSearchChange` props we still need to pass them through because it's the parent component to `RecipeSearch`. Imagine more deeply nested components trees, then you can already see chaos. Libraries like Redux helps solve this problem but context is a simple lightweight version.

<img src= "https://cdn.hashnode.com/res/hashnode/image/upload/v1562193246399/6iH63N_v0.png" style="display: block;margin-left: auto;margin-right: auto;width: 70%;  >



##  *useContext* hook to the rescue

So what if we could access whatever state we want higher up within the component tree and just use them only where they are needed. This is what we are going to do next with `useContext hook`.  There are a couple of things to understand with context API.
- Context is designed to share data that can be considered “global” for a tree of React components.
- Context is primarily used when some data needs to be accessible by many components at different nesting level

Let's discuss the API that context gives us. First we have `React.createContext`. It create a Context object. When React renders a component that subscribes to this Context object it will read the current context value from the closest matching Provider above it in the tree.

`Context.Provider` - Every Context object comes with a Provider React component that allows consuming components to subscribe to context changes.

`Context.Consumer` - A React component that subscribes to context changes. This lets you subscribe to a context within a function component.


## Let's bundle all this into our example and see how it works.

First, let's create a context folder or just a file with and index.js file in it. Inside `index.js` we need to create a `Provider` that accepts and provides the data to all children components within it. First, let's move all our data fetching logic from `App.js` into the context file. You should be left with an almost empty `App.js` like below.

{{< highlight jsm  "linenostart=1"  >}}
import React, { useContext } from 'react';
import RecipeList from './components/RecipeList'
function App() {

  return (
    <div>
      {loading ? <h1 className="text-center">...fetching {search} recipe</h1> :
        <RecipeList />}
    </div>
  );
}
export default App;

{{</ highlight >}}

And  `index.js` ...

{{< highlight jsm  "linenostart=1"  >}}
import React, { useState, useEffect } from 'react'

const RecipeContext = React.createContext()

const apiKey = `7cdab426afc366070dab735500555521`

const RecipeProvider = (props) => {
  let url = `https://www.food2fork.com/api/search?key=${apiKey}`
  const [showHomeButton, setShowHomeButton] = useState(false)
  const [recipes, setRecipes] = useState([])
  const [loading, setLoading] = useState(true)
  const [search, setSearch] = useState('')

  const fetchRecipe = async () => {
    try {
      const recipeData = await fetch(url)
      const { recipes } = await recipeData.json()
      setRecipes(recipes)
      setLoading(false)
      console.log(recipes)
    } catch (e) {
      if (e) {
        console.log(e.message, 'Try updating the API key in App.js')
      }
    }
  }
  const handleSubmit = async (e) => {
    e.preventDefault()
    try {
      setLoading(true)
      const searchUrl = `${url}&q=${search}`
      const searchedRecipeData = await fetch(searchUrl)
      const { recipes } = await searchedRecipeData.json()
      setRecipes(recipes)
      setLoading(false)
      setShowHomeButton(true)
    } catch (e) {
      console.log(e)
    }
  }
  const handleSearchChange = (e) => {
    setSearch(e.target.value)
  }
  const handleReturnHome = () => {
    fetchRecipe()
    setShowHomeButton(false)
  }

  useEffect(() => {
    fetchRecipe()

  }, [])

  return (
    <RecipeContext.Provider value={{
      loading,
      search,
      showHomeButton,
      recipes,
      handleSearchChange,
      handleSubmit,
      handleReturnHome,
    }}>
      {props.children}
    </RecipeContext.Provider>
  )
}

export { RecipeProvider,  RecipeContext }

{{</ highlight >}}

### Let's try to understand the above code by taking it apart carefully.

We have simply moved all the logic for data fetching from our `App.js` into `index.js` so as to make it available globally. We already know the drill, we have the data and the states just like we did before.

Now, to the interesting part, on line 2. We have created the `RecipeContext` from `React.createContext()` method. RecipeContext is just a variable, the name is not relevant, it is simply good practice to give it a name close to what it does. When we created the `RecipeContext` it gives us access to 2 other properties namely the `Provider` and the `Consumer`. The **Provider** gives us a way to pass all the data and states as props from the topmost part of React tree to wherever we would like to consume it. Hence we created `RecipeContext.Provider` and passed down all states and data as a prop called **value **.  We then export these values to be used. Like below. It will become clearer when we begin to use them.

{{< highlight jsm  "linenostart=1"  >}}
 <RecipeContext.Provider value={{
      loading,
      search,
      showHomeButton,
      recipes,
      handleSearchChange,
      handleSubmit,
      handleReturnHome,
    }}>
      {props.children}
    </RecipeContext.Provider>
  )
}

export { RecipeProvider,  RecipeContext }
{{</ highlight >}}
Next, we have to locate the highest tree in our component hierarchy to pass the **value** props to. It's only through this way that it can be able to pass it along to all its children. And that would be the `index.js` in our root. This is where the `App` component is located. Hence, the App component is able to pass any data or state as props to wherever it will be needed. In the `index,js` in the root of your application. You should wrap the `RecipeProvider` around App like below.

{{< highlight jsm  "linenostart=1"  >}}
import React from 'react';
import ReactDOM from 'react-dom';
import { RecipeProvider } from './context/index'

ReactDOM.render(
  <RecipeProvider>
    <App />
  </RecipeProvider>
  , document.getElementById('root'));

{{</ highlight  >}}
From here henceforth all the props pass down inside our context file is available to all the children of `App` component, which is essentially every component. The exciting part is how we'll then use this. We'll make an example with `RecipeList` and `RecipeSearch` components. Your **RecipeList** component should look like below.

{{< highlight jsm  "linenostart=1"  >}}
import React, { useContext } from 'react'
import Recipe from './Recipe'
import RecipeSearch from './RecipeSearch'
import { RecipeContext } from '../context/index'

export default function RecipeList() {
  const appContext = useContext(RecipeContext)
  const { showHomeButton, recipes, handleReturnHome } = appContext

  return (
    <>
      <RecipeSearch></RecipeSearch>
      <div className="container my-5 home-button">
        {showHomeButton && <button type="button"
          className="btn btn-warning"
          onClick={() => handleReturnHome()}>
          Go Back Home
        </button>}
        <div className=" d-flex d-flex justify-content-center mb-3">
          <h1 className="text-slaned ">Recipe List</h1>
        </div>

        <div className="row recipe-list">
          {recipes.map(recipe => {
            return <Recipe
              key={recipe.recipe_id} recipe={recipe} />
          })}
        </div>
      </div>
    </>
  )
}
{{</ highlight >}}

We have now imported `RecipeContext` from our file and imported `useContext` from React. Inside our function, we created a variable to hold the value of RecipeContext. We then just grab only the values we need inside **RecipeList**. This is so clean in many ways, 2 of which are.
- First, we just went to `RecipeList` straight where we needed those states and data, we didn't have to thread the props from parent to children.
- Second, we only extracted the props we needed within the component, we didn't need to give `RecipeList` some props it didn't need but just has to pass along to its children. We just go to that child component directly if they needed the data.

Let's examine `RecipeSearch` component. Once again, let's see the code and then explain it.

{{< highlight jsm  "linenostart=1"  >}}
import React, { useContext } from 'react'
import { RecipeContext } from '../context/index'

export default function RecipeSearch() {
  const appContext = useContext(RecipeContext)
  const { handleSubmit, handleSearchChange } = appContext

  return (
    <div className="container d-flex justify-content-center my-5">
      <form className="form-inline" onSubmit={(e) => handleSubmit(e)}>
        <label htmlFor="search" >Search:</label>
        <input data-age="Love" onChange={(e) => handleSearchChange(e)} type="text" className="form-control" id="search" placeholder="coffeee" name="search" />
        <button type="submit" className="btn btn-primary"><i className="fa fa-search"></i></button>
      </form>
    </div>
  )
}
{{</ highlight >}}
Like in the **RecipeList** we  have imported `useContext` and **RecipeContext**. We created the variables. And just see how clean and concise this is, we didn't have to get `handleSubmit` and `handleSearchChange` from **RecipeList** we just simply grabbed them from the context here.

And that's just about it. We have covered 3 major types of hooks. *useState*, *useEffect* and *useContext*. I believe these are the foundation necessary to understand more advanced and better ways to work with react hooks. I believe they are cleaner and easier to digest for a beginner.

Moving forward, if you'd like to gain a deeper understanding. You can refactor the `Recipe` component to use the `useContext`. Take an old project with a class component and convert it into a functional component just using hooks. If you want something more challenging, learn the `useReducer` hook and how we can refactor multiple states in our **context** file to make use of **useReducer**

Thank you!. The full code can be found on [github](https://github.com/olajohn-ajiboye/Blog-React-Hook-Tutorial). Comments and feedback are welcomed.
