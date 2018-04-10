## [Jest](https://facebook.github.io/jest/)

Jest is a test runner that makes testing front-end applications (can also test [nodeJs](nodejs.org) apps) much more easier as it takes care of a bunch of configurations steps which could be painful to implement.

With create-react-app we don't have to install Jest as it is already bundled with it.

All we have to do is writing our tests following some naming conventions (under the `src` folder):

* Files with .js suffix in \_\_tests\_\_ folders.
* Files with .test.js suffix.
* Files with .spec.js suffix.

In the test files we can add `it()` or `test()` blocks to create a test.
Jest also supports `describe()` blocks to group the tests by functionality.

To make our assertions Jest provide us with the built-in global function `expect()` which supports a lot of matchers such as `toEqual`, `toBe`... [check the documentation to see the full list](https://facebook.github.io/jest/docs/en/expect.html).

[Further Reading](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#running-tests)

## [Enzyme](http://airbnb.io/enzyme/)

Enzyme is a JavaScript Testing utility built by airbnb specially for for React. Enzyme simplifies testing React component as it provides us with an easy to use API to make assertions about the component's implementation.

### Installation

To be able to use Enzyme we have to install it with it's React 16 adapter:

```bash
yarn add --dev enzyme enzyme-adapter-react-16
```

After that create a new file in the `src` directory and name it `setupTests.js` with the following content:

```js
import { configure } from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';

configure({ adapter: new Adapter() });
```

### Usage

Let's say that we have a `Button` component with the following code in it:

```js
import React from 'react';

export default ({ disabled, text, onClick }) => (
  <button onClick={onClick} className={disabled ? 'button-disabled' : ''}>
    {text}
  </button>
);
```

This component can be tested with Enzyme like this:

```js
import React from 'react';
import { shallow } from 'enzyme';

import Button from './Button';

describe('Button', () => {
  test('Button behaviour', () => {
    const wrapper = shallow(
      <Button disabled text="click me" onClick={jest.fn()} />
    );

    expect(wrapper.hasClass('button-disabled')).toEqual(true);
  });
});
```

Note that `shallow` renders only the wrapped component without it's child components, if you need a full mount you can use `mount` from Enzyme which has the same API as `shallow`

```js
import { mount } form 'enzyme';
```
