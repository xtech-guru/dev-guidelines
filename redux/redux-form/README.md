# [Redux-form](https://redux-form.com/7.2.3/docs/gettingstarted.md/)

## Installation

```bash
yarn add redux-form
```

```diff
import { combineReducers } from 'redux';
+ import { reducer as formReducer } from 'redux-form';

import app from './modules/app/app.ducks';
import auth from './modules/auth/auth.ducks';

const rootReducer = combineReducers({
  app,
-  auth
+  auth,
+  form: formReducer
});

export default rootReducer;
```

## Basic usage

```js
import React from 'react';
import { Field, reduxForm } from 'redux-form';

const EventForm = ({ handleSubmit }) => (
  <form onSubmit={handleSubmit}>
    <div>
      <label>Event Name</label>
      <Field name="eventName" component="input" type="text" />
    </div>
    <div>
      <label>Event Description</label>
      <Field name="eventDescription" component="input" type="text" />
    </div>
    <button type="submit">add</button>
  </form>
);

const createEventForm = reduxForm({ form: 'event' });

const EventFormContainer = createEventForm(EventForm);

class AddEventPage extends React.Component {
  addEvent = values => {
    this.props.addEvent(values);
  };

  render() {
    return <EventFormContainer onSubmit={this.addEvent} />;
  }
}

class UpdateEventPage extends React.Component {
  updateEvent = values => {
    this.props.updateEvent(values);
  };

  render() {
    return (
      <EventFormContainer
        onSubmit={this.updateEvent}
        // passe initialValues object prop to populate the form
        initialValues={this.props.event}
      />
    );
  }
}
```

## Use custom Fields

```diff
+const InputField = ({ input, label, type, meta }) => (
+  <div>
+    <label>{label}</label>
+    <input {...input}  type={type} />
+  </div>
+);

const EventForm = ({ handleSubmit }) => (
  <form onSubmit={handleSubmit}>
-    <div>
-      <label>Event Name</label>
-      <Field name="eventName" component="input" type="text" />
-    </div>
+    <Field name="eventName" component={InputField} type="text" />
-    <div>
-      <label>Event Description</label>
-      <Field name="eventDescription" component="input" type="text" />
-    </div>
+    <Field name="eventDescription" component={InputField} type="text" />
    <button type="submit">add</button>
  </form>
);
```

## Form Validation

```diff
const InputField = ({ input, label, type, meta }) => (
  <div>
    <label>{label}</label>
    <input {...input}  type={type} />
+    <div>{meta.touched && meta.error}</div>
  </div>
);

+const validate = (values) => {
+  errors = {};
+
+  if (!values.eventName) errors.eventName = 'required';
+  if (!values.eventDescription) errors.eventDescription = 'required';
+
+  return errors;
+}

-const createEventForm = reduxForm({ form: 'event' });
+const createEventForm = reduxForm({ form: 'event', validate });
```
