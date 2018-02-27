## Installation

To add [redux](https://redux.js.org/) to a React project you must install `redux` itself and it's [`React` bindings](https://github.com/reactjs/react-redux):

```bash
yarn add redux react-redux
```

You can also use redux middlewares such as [redux-thunk](https://github.com/gaearon/redux-thunk) and [redux-promise-middleware](https://github.com/pburtchaell/redux-promise-middleware).

```bash
yarn add redux-promise-middleware redux-thunk
```

In the `src/` directory you have to create a new file that will hold the root reducer of the application, and let's name it `reducers.js`.

The content of `reducers.js` will look something like the following:

```javascript
import { combineReducers } from 'redux';

// here you import your sub-reducers.
import app from './modules/app/app.ducks';
import auth from './modules/auth/auth.ducks';

const rootReducer = combineReducers({
  app,
  auth
});

// don't forget to export the root reducer.
export default rootReducer;
```

In the entry point of the application (in our case `src/index.js`):

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import promiseMiddleware from 'redux-promise-middleware';
import thunk from 'redux-thunk';
import { createStore, applyMiddleware } from 'redux';

import App from './modules/app/App';
import rootReducer from './reducers';

const middlewares = applyMiddleware(thunk, promiseMiddleware());

const store = createStore(rootReducer, middlewares);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

If we want to enable the [redux-devtools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd) for better debugging experience we can add:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import promiseMiddleware from 'redux-promise-middleware';
import thunk from 'redux-thunk';
import { createStore, applyMiddleware, compose } from 'redux';

import './index.css';
import App from './modules/app/App';
import { initApp } from './modules/app/app.ducks';
import rootReducer from './reducers';
import registerServiceWorker from './registerServiceWorker';

const isDev = process.env.NODE_ENV !== 'production';

const middleware = applyMiddleware(thunk, promiseMiddleware());
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const store = createStore(
  rootReducer,
  isDev ? composeEnhancers(middleware) : middleware
);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
```

## Best Practices

### [Ducks](https://medium.freecodecamp.org/scaling-your-redux-app-with-ducks-6115955638be)

In our applications we use the commonly used `ducks` pattern:
All our redux logic should be located in the `src/modules` directory. Each module should have one file with the extension `.ducks.js` within this file we can declare our "sub-reducers", actions, and action creators.
Let's say that we have a todo application and for simplicity we have one action 'ADD_TODO':

```javascript
// actions.
const ADD_TODO = 'ADD_TODO';

// reudcer
const todos = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload.todo];
    default:
      return state;
  }
};

// the reducer must be the default export.
export default todos;

// action creators
export const addTodo = todo => ({ type: ADD_TODO, payload: { todo } });
```

### Defining state shape and advanced usage of [`combineReducers`](https://redux.js.org/recipes/structuring-reducers/using-combinereducers)

From the redux documentation:

> The most common state shape for a Redux app is a plain Javascript object containing "slices" of domain-specific data at each top-level key. Similarly, the most common approach to writing reducer logic for that state shape is to have "slice reducer" functions, each with the same (state, action) signature, and each responsible for managing all updates to that specific slice of state. Multiple slice reducers can respond to the same action, independently update their own slice as needed, and the updated slices are combined into the new state object.

> Because this pattern is so common, Redux provides the combineReducers utility to implement that behavior. It is an example of a higher-order reducer, which takes an object full of slice reducer functions, and returns a new reducer function.

Following that we can rewrite our duck:

```diff
+ import { combineReducers } from 'redux';
+
// actions.
const ADD_TODO = 'ADD_TODO';

+ const allIds = (state = [], action) => {
+  switch(action.type) {
+    case ADD_TODO:
+      return [...state, action.payload.todo.id];
+    default:
+      return state;
+  }
+}
+
+ const byId = (state = {}, action) => {
+  switch(action.type) {
+    case ADD_TODO:
+      return {
+        ...state,
+        [action.payload.todo.id]: action.payload.todo
+      };
+    default:
+      return state;
+  }
+}

// reudcer
+ const todos = combineReducers({ allIds, byId });
- const todos = (state = [], action) => {
- switch (action.type) {
-    case ADD_TODO:
-      return [...state, action.payload.todo];
-    default:
-      return state;
-  }
-};

// the reducer must be the default export.
export default todos;

// action creators
export const addTodo = todo => ({ type: ADD_TODO, payload: { todo } });
```

We may call combineReducers at any level of the reducer hierarchy. It doesn't have to happen at the top. In fact we may use it again to split the child reducers that get too complicated into independent grandchildren, and so on.

This approach has several benefits:

* makes the state manipulation centralized each slice of state in its own reducer.
* adding new actions becomes more easy.
* makes debugging easier as we can isolate in any level exactly the issue happens.
* the initialization of the state becomes less painful as we don't have to provide all the initial state at once and each time we want to add a piece of state we initialize it at its reducer without need to change the initial state object (which is error prone).
* simplifies the unit test of each reducer.
* take advantage of the ES2015 object shorthand syntax to provide more readable state shape.
