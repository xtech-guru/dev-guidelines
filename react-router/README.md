# Installation

To add react-router to a `React` project we have just to add it to our dependencies.

```bash
yarn add react-router-dom
```

After that we can declare the routes of the application, In the component which will wrap the routes (here `src/modules/app/App.js`):

```js
import React, { Component } from 'react';
import {
  BrowserRouter as Router,
  Route,
  Switch,
  Redirect
} from 'react-router-dom';

import PrivateRoute from '../../components/PrivateRoute';
import Landing from '../landing/Landing';
import ProductsList from '../Products/ProductsList';
import Product from '../Products/Product';

export class App extends Component {
  render() {
    return (
      <Router>
        <Switch>
          <Route path="/" exact component={Landing} />
          <PrivateRoute path="/products" exact component={ProductsList} />
          <PrivateRoute path="/products/:id" component={Product} />
          <Route render={props => <Redirect to="/" />} />
        </Switch>
      </Router>
    );
  }
}

export default App;
```

The `PrivateRoute` component in the code snippet above is a component that checks if the user has the right to access to specified route instead it will redirect it to another route (can be the login page). It's code can be something like the following:

```js
import React, { Component } from 'react';
import { Route, Redirect } from 'react-router-dom';

class PrivateRoute extends Component {
  render() {
    const { component: Component, isAuthenticated, ...rest } = this.props;

    return (
      <Route
        {...rest}
        render={props =>
          isAuthenticated ? <Component {...props} /> : <Redirect to="/login" />
        }
      />
    );
  }
}

export default PrivateRoute;
```
