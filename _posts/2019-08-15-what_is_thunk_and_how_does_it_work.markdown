---
layout: post
title:      "What is Thunk and How Does it Work?"
date:       2019-08-15 16:31:40 +0000
permalink:  what_is_thunk_and_how_does_it_work
---


Redux-Thunk can be a challenging concept to wrap one's head around at first. However, once you get the hang of it it's rather straight forward. As a prerequisite to understanding Redux, one must have a solid understanding of React-Redux first. What Thunk does is allow us to place asynchronous web requests using JSX fetch and promise. What is an asynchronous request you may ask? Well it is important to recognize that React utilizes asynchrounous rendering, especially when utilizing lifestyle components such as componentDidMount. This means that the DOM will be automatically refreshed without the user needing to reload the page, making the app function extremely fast. However, as we know, web requests are do not yield immediate results and depending on the server, may take some time. If we didn't incorporate Thunk into our React-Redux app, everything would break immediately because the asynchronous rendering requires content to render, and when that content comes as the response of the request, if the response is absent, it will inevitiably break. 

This is where Thunk comes in. If we utilize fetch  to make an API request it will return a 'promise', which is simply an object that *promises* results will yield at a later time. Once the promise is resolved we will then be able to access it. One should set up these requests by chaining on a `.then` to the `fetch()` call so that our code knows what to do once the promise value updates with results from the request. Our fetch might look something like this:

```
fetch('http://example.com/movies.json')
  .then(function(response) {
    return response.json();
  })
  .then(function(myJson) {
    console.log(JSON.stringify(myJson));
  });
	```
	
	In order for our application to not break, as described above, we will need to incoporate Thunk middleware into our project, which will dispatch an action after invoking the fetch() stating that we are loading the data, then after the response is recieved, dispatching another action containing the data from the response. After installing Redux-Thunk we will want to import `applyMiddleware` from Redux and add it to the store. Your `Index.js` will look something like this:
	
	```
	import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
import rootReducer from './reducers';
 
const store = createStore(rootReducer, applyMiddleware(thunk));
 
ReactDOM.render(
  <Provider store={store} >
    <App />
  </Provider>, document.getElementById('container')
)
	```
	
Without Redux-Thunk the action creator returns a POJO (Plain Old Javascript Object), but with Thunk middleware it will return a function that takes the dispatch from the store as its argument, allowing us to dispatch actions from within the function itself. Our action file may look something like this:
	
	```
	export function fetchMovies() {
  return (dispatch) => {
    dispatch({ type: 'START_ADDING_MOVIES_REQUEST' });
    return fetch('http://example.com/movies.json')
      .then(response => response.json())
      .then(movies => dispatch({ type: 'ADD_MOVIES, movies }));
  };
}
```

In summary, Redux-Thunk is a middleware that allows us to make asynchronous API requests in our React-Redux apps to seamlessly render data from the request without any disruption to the user or needing to reload the page. 
