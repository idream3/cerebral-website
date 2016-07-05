## Operators

Most state changes are quite simple. Usually you want to set some new state, toggle it or maybe copy a value from the input of the signal into the model. With the included operators you can do this directly in the chain definition, and more.

```javascript
import {
  set,
  copy,
  toggle,
  unset,
  when,
  throttle,
  debounce,
  filter
} from 'cerebral/operators'

export default [
  // Set some value to the model
  set('state:foo.bar', 'someValue'),
  // Set some value to the output
  set('output:foo.bar', 'someValue'),

  // Copy value from input to model
  copy('input:foo', 'state:bar'),
  // Copy value from model to output
  copy('state:foo', 'output:bar'),

  // Toggle a boolean value in your model
  toggle('state:foo'),

  // Unset a value from the model
  unset('state:foo.bar'),

  // Conditional truthy check
  when('state:foo.isAwesome'), {
    true: [],
    false: []
  },
  // Conditional value check
  when('state:user.role', {
    admin: 'admin',
    user: 'user'
  }), {
    admin: [],
    user: [],
    otherwise: []
  },

  // Go down "accepted" path the first time
  // or if 200ms has passed since last "accepted"
  // Go down "discarded" path when 200ms has not
  // passed since last "accepted"
  throttle(200), {
    accepted: [],
    discarded: []
  },

  // This action holds until
  // A: 200ms has passed
  //  - "accepted" is run
  // B: the action is triggered again
  //  - "discarded" is run on the previous execution
  //  - the action on new execution is holding again
  debounce(200), {
    accepted: [],
    discarded: []
  },

  // Go down "accepted" when value matches filter
  // or "discarded" when it does not match
  filter('input:foo', function minLength3(value) {
    return value.length >= 3
  }), {
    accepted: [],
    discarded: []
  },
]
```