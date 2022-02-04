# useState

**useState** is a React Hook that manager state in ract functional component.  It returns a stateful value and a function to update the state. `useState` can only be use in functional component not in class component. 

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
### Lazy initial state

If you are giving initial state a constant value then `const [count, setCount] = useState(InitialState())` is ok. But if the initial state of the state is a result of more complex operation then it will really slow down the performance. Fore eg.

 ```javascript

const InitialState = ()=> {
    console.log("Initializing State");
    return 10
    
  }
const [count, setCount] = useState(InitialState())

 ``` 

 Here every time you change the `count` value `InitialState()` is called. To avoid such exensive process we can pass `function` as initial state in `useState`. For eg.

 ```javascript
const InitialState = ()=> {
    console.log("Initializing State");
    return 10
    
  }
const [count, setCount] = useState(()=>InitialState())
  
 ```

 Here, now InitialState() is called only once.



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

## Types of State Variable

- State can take following variable to work with
  - boolean
  - string
  - number
  - array
  - object


Examples

```javascript 
const [count, setCount] = useState(0)
const [color, setColor] = useState('#526b2d')
const [isHidden, setIsHidden] = useState(true)
const [products, setProducts] = useState([])
const [user, setUser] = useState({
    username: '',
    avatar: '',
    email: '',
})
```

## Updating State Variable

For Array
```javascript
onst [items, setItems] = useState([])

// Completely replaces whatever was stored in the items array
setItems([{item1}, {item2}])

// Instead, make a copy of the array then add your new item onto the end
setItems([...items, {item3}])

// To update an item in the array use .map. 
// Assumes each array item is an object with an id.
setItems(
  items.map((item, index) => {
    item.id === id ? newItem : item
  })
)
```


For Object

```javascript

const [person, setPerson] = useState({
    firstName: 'rajan',
    lastName: 'twn'
  });

   const updatePerson = (e) => {
    setPerson({
      ...person,
      {person.firstName, person.lastName: "twanabashu"}
    });
  };
```

For  Array of object

```javascript
const [people, setPeople] = useState({
  jerry: {
    firstName: 'Jerry',
    lastName: 'Garcia',
    address: {
      street: '710 Ashbury Street',
      city: 'San Francisco',
      state: 'CA',
      zip: '94117'
    }
  },
  jim: {
    firstName: 'Jim',
    lastName: 'Morrison',
    address: {
      street: '8021 Rothdell Trail',
      city: 'Los Angeles',
      state: 'CA',
      zip: '90046'
    }
  }
})

// Jerry is gonna move next door
setPeople({
  // Copy people
  ...people,
  // Overwrite person you want to update
  jerry: {
    // Copy Jerry's existing properties
    ...people.jerry,
    // Overwrite Jerry's address  
    address: {
      // Copy everything over from Jerry's original address
      ...people.jerry.address,
      // Update the street
      street: '712 Ashbury Street'
    }
  }
})
```

[Refrence Link] (https://www.youtube.com/watch?v=O6P86uwfdR0&list=PLZlA0Gpn_vH8EtggFGERCwMY5u5hOjf-h&index=1)
