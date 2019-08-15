---
layout: post
title:      "What is React-Redux and How Does it Work?"
date:       2019-08-15 12:06:40 -0400
permalink:  what_is_react-redux_and_how_does_it_work
---


Once you have a solid understanding of React you should be ready to take on Redux, a necessary aspect of developing in React! Redux can be a little tricky to grasp so we will go over the basics here. Simply put, Redux provides us an elegant way to manage various states throughout an application. With an extremely simple app you may not need Redux, but as soon as you start to add any complexity you'll definitely want to consider it, as Redux offers us a library that allows us to track the various states in a safe and reliable manner.

Redux houses our various states within what is known as the "store". The store will most likely live within Index.js. You will need to import the store from React-Redux then create and define a constant called store, which takes a function as the first argument and then a reducer. Optionally you may pass 'store' an initial state.

```
// src/js/store/index.js
import { createStore } from "redux";
import rootReducer from "../reducers/index";
const store = createStore(rootReducer);
export default store;
```


Next we'll take a look at our reducers, which produce the state of the application. A reducer is simply a JS function that takes in the current state plus an action. Additionally, the reducer is a 'pure function', accepting an input and returning a value that does not modify any data outside its scope. Using the switch statement, when an action.type matches, the reducer determines the next state and returns a new object. It is critical in the return to not mutate the original state, therefore we recommend using concat, splice  and ...spread for arrays and Object.assign() and ...spread for objects. It is always best practice to return the initial state if no action type matches the cases in the reducer's switch statement. Here you will see a sample reducer:

```
export default function todos(state = [], action) {
  switch (action.type) {
    case 'ADD_TODO':
      return state.concat([action.text])
    default:
      return state
  }
}
```

In order to change the state we must dispatch an *action* to the store. The action is a payload that requires a 'type' that describes how the state should change. It will look something like this:

```
{
  type: ADD_TODO,
  payload: { title: 'Build React App', id: 1 }
}
```

We will then wrap the action within a function known as the 'action creator' and these actions live inside of an action file:

```
// src/js/actions/index.js

export const addTodo(payload) {
  return { type: "ADD_TODO", payload }
};
```

It's best practice to store the type property as a Const instead of simply a string, as the reducer will use it to determine the next state.

Simply put, once the action is dispatched to the store, the store triggers the reducer which returns the new state.

Now, in order for React and Redux to play together we'll need to import 'connect', connecting the desired React component to Redux. Additionally, we'll need to wrap 'Provider' around the React application providing the entire app access to the Redux store. It will look something like this:

```
import React from "react";
import { render } from "react-dom";
import { Provider } from "react-redux";
import store from "./store/index";
import App from "./components/App.jsx";

render(
  <Provider store={store}>
    <App />
  </Provider>,

  document.getElementById("root")
);
```

Within the component we'd like to interact with the store, we'll want to import connect as previously mentioned `import { connect } from "react-redux";`. We will then use the function MapStateToProps to extract the desired state from Redux to the React component as a prop. We'll use MapDispatchToProps function to allow the React component to dispatch actions to the Redux store. Lastly, don't forget to export default your connect, including your MapStateToProps/MapDispatchToProps (optional) along with the app component itself. 
