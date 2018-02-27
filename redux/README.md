# Installation

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
