# useEffect

`useEffect` is one of the most useful react hook.  `useEffect` helps to execute some form of side effect  when ever some thing happens.

`side-effect`: A functional React component uses props and/or state to calculate the output. If the functional component makes calculations that don’t target the output value, then these calculations are named side-effects.

### useEffect Defination

`import {useEffect} from 'react`

`useEffect(callback, [, dependencies])`

- `callback` is the callback function containing side-effect logic. useEffect() executes the callback function after React has committed the changes to the screen.
- `dependencies`is an optional array of dependencies. useEffect() executes callback only if the dependencies have changed between renderings.


`dependencies` argument of useEffect(callback, dependencies) lets you control when the side-effect runs. When dependencies are:


### Not provided: the side-effect runs after every rendering.

```javascript
import { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    // Runs after EVERY rendering
  });  
}
```

### An empty array []: the side-effect runs once after the initial rendering.

Kind of componentDidount

```javascript
import { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    // Runs after EVERY rendering
  }, []);  
}
```


### Has props or state values [prop1, prop2, ..., state1, state2]: the side-effect runs only when any depenendecy value changes.

```javascript
import { useEffect } from 'react';

const [stateVariable, setStateVariable] = useState(false)

function MyComponent({ prop }) {
  useEffect(() => {
    // Runs ONCE after initial rendering
    // and after every rendering ONLY IF `prop` changes
  }, [stateVariable]);
```

Here useEffect will run only when the actually stateVariable changes. 

## The side-effect cleanup

There are side-effects that need cleanup: close a socket, clear timers. If the callback of useEffect(callback) returns a function, then useEffect() considers this as an effect cleanup:

```javascript
useEffect(() => {
  // Side-effect...

  return function cleanup() {
    // Side-effect cleanup...
  };
}, dependencies);
```