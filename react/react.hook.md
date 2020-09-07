# useState

**useState** is a React Hook that manager state in ract functional component.  It returns a stateful value and a function to update the state.

        const [count, setCount] = useState(initialState)

- To read state

        {count}

- To update the sate

        setCount(newValue)

During the initial render the return state value is equal to the initial state passed. Once the value is update via **setCount** it re-render the compont to reflect the change. React guarantees that setState function identity is stable and won’t change on re-renders.

<br>
In below example when the user click on the Counter button it increment the count by 1. On count value update it react re-render to reflect the new state.

```js
function App() {
  const [count, setCount] = useState(0)

  const increment = ()=>{
    setCount(count +1)
  }
  return (
    <View>
      <button onClick={increment}> Counter {count}</button>
      </View>
  );
}
```

### Using previousState

The above code snippet is not a safest way to update the state. Instead we should pass previousState to **setCount** so that it  can clone the state object before it actually update.

```js
export default function Counter() {
  const initialCount = 0;
  const [count, setCount] = useState(initialCount);

  
  return (
    <View>
      <Text>Count: {count} </Text>
      <button onClick={() => setCount(previousCiubt => previousCiubt + 1)}> increment</button>
    </View>
  );
}

```

### useState with array or dictionary

When using useState with array or dictionary, we first need to copy whole object and then only modify the object or object using **spread operator.** {**...name**}

```js
import React, { useState } from "react";
import { View, Text } from "react-native";

export default function Counter() {
  const [name, setName] = useState({ firstname: "", lastname: "" });

  return (
    <View>
      <Text>
        Your name is {name.firstname} - {name.lastname}{" "}
      </Text>
      <form>
        <input
          type="text"
          value={name.firstname}
          onChange={(e) => setName({...name,  firstname: e.target.value })}
        />
        <input
          type="text"
          value={name.lastname}
          onChange={(e) => setName({...name,  lastname: e.target.value })}
        />
      </form>
    </View>
  );
}
```
