
# Optimizing a React App with Memoization and Component Design

In this blog post, we'll walk through creating a simple React application that lists countries and includes a button to toggle between dark and light modes. We'll start with a basic setup, identify performance issues, and then optimize the application using React.memo, useMemo, and proper component design.

## Initial Setup

Let's start by setting up a basic React application. We'll create two components: CountryList to display a list of countries and CountryForm to add new countries to the list. Additionally, we'll include a button to toggle the background color.

Here's the initial code:

```javascript
import React, { useState } from 'react';
import ReactDOM from 'react-dom';

const App = () => {
  console.log("App re-render")  
  const [countries, setCountries] = useState(['USA', 'Canada', 'Mexico']);
  const [newCountry, setNewCountry] = useState('');
  const [isDarkMode, setIsDarkMode] = useState(false);

  const addCountry = () => {
    if (newCountry) {
      setCountries([...countries, newCountry]);
      setNewCountry('');
    }
  };

  const toggleBackgroundColor = () => {
    setIsDarkMode(!isDarkMode);
  };

  return (
    <div
      style={{
        backgroundColor: isDarkMode ? '#333' : '#FFF',
        color: isDarkMode ? '#FFF' : '#000',
        minHeight: '100vh',
        padding: '20px',
      }}
    >
      <h1>Country List</h1>
      <ul>
        {countries.map((country, index) => (
          <li key={index}>{country}</li>
        ))}
      </ul>
      <input
        type="text"
        value={newCountry}
        onChange={(e) => setNewCountry(e.target.value)}
        placeholder="Add a country"
      />
      <button onClick={addCountry}>Add Country</button>
      <br />
      <button onClick={toggleBackgroundColor}>
        Toggle Background Color
      </button>
    </div>
  );
};



```

## Identifying the Problem

While this setup works, we quickly notice that the entire application re-renders whenever we add a new country or toggle the background color. This is evident from the console logs.

This leads to unnecessary re-renders and potential performance issues, especially as the application scales.

## Optimizing with Memoization and Callbacks

To address these performance concerns, we can optimize the application using React.memo, useMemo, and useCallback.

1. Creating seperate component and Memoizing CountryList

By memoizing CountryList, we ensure it only re-renders when the countries prop changes.

```javascript 
import React, { memo } from 'react';

const CountryList = memo(({ countries }) => {
  console.log('Rendering CountryList');
  return (
    <ul>
      {countries.map((country, index) => (
        <li key={index}>{country}</li>
      ))}
    </ul>
  );
});

```

2. Creating seperate component and  Memoizing CountryForm

We can also memoize CountryForm. Though it doesn't involve heavy computations, memoizing it ensures it only re-renders when its props change, which is beneficial as the app grows.


```javascript
const CountryForm = memo(({ addCountry }) => {
  console.log('Rendering CountryForm');
  const [newCountry, setNewCountry] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (newCountry) {
      addCountry(newCountry);
      setNewCountry('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={newCountry}
        onChange={(e) => setNewCountry(e.target.value)}
        placeholder="Add a country"
      />
      <button type="submit">Add Country</button>
    </form>
  );
});

```

3. Using useCallback for the addCountry Function

Using useCallback, we can memoize the addCountry function to ensure it remains stable across renders unless the setCountries function changes.


```javascript
const addCountry = useCallback(
  (country) => {
    setCountries((prevCountries) => [...prevCountries, country]);
  },
  [setCountries]
);

```

## Final Optimized Code

Here's the complete optimized code:

```javascript
import React, { useState, useCallback, memo } from 'react';
import ReactDOM from 'react-dom';
import './App.css';

const CountryList = memo(({ countries }) => {
  console.log('Rendering CountryList');
  return (
    <ul>
      {countries.map((country, index) => (
        <li key={index}>{country}</li>
      ))}
    </ul>
  );
});

const CountryForm = memo(({ addCountry }) => {
  console.log('Rendering CountryForm');
  const [newCountry, setNewCountry] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (newCountry) {
      addCountry(newCountry);
      setNewCountry('');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={newCountry}
        onChange={(e) => setNewCountry(e.target.value)}
        placeholder="Add a country"
      />
      <button type="submit">Add Country</button>
    </form>
  );
});

const App = () => {
  console.log('Rendering App');
  const [countries, setCountries] = useState(['USA', 'Canada', 'Mexico']);
  const [isDarkMode, setIsDarkMode] = useState(false);

  const addCountry = useCallback(
    (country) => {
      setCountries((prevCountries) => [...prevCountries, country]);
    },
    [setCountries]
  );

  const toggleBackgroundColor = () => {
    setIsDarkMode(!isDarkMode);
  };

  return (
    <div
      style={{
        backgroundColor: isDarkMode ? '#333' : '#FFF',
        color: isDarkMode ? '#FFF' : '#000',
        minHeight: '100vh',
        padding: '20px',
      }}
    >
      <h1>Country List</h1>
      <CountryList countries={countries} />
      <CountryForm addCountry={addCountry} />
      <br />
      <button onClick={toggleBackgroundColor}>Toggle Background Color</button>
    </div>
  );
};

ReactDOM.render(<App />, document.getElementById('root'));

```

Here's still one issue, every time when we add country app re-render twice. 

```console
2 Rendering App
2 Rendering CountryList
2 Rendering CountryForm
```

# Explanation of Double Rendering

In React's strict mode, some lifecycle methods (like componentDidMount and componentDidUpdate) and state updates are invoked twice to ensure that your components handle side effects safely and correctly. This behavior can make it seem like your components are rendering more times than necessary. This only happens in development mode and not in production mode.

To confirm this, you can remove or disable strict mode and see if the issue persists.

# Confirming Double Rendering in Development Mode

Strict Mode: Check if your application is wrapped in React.StrictMode. If so, this could be the reason for the double rendering.
Remove Strict Mode: Temporarily remove React.StrictMode to see if the double rendering issue persists.
Here's an example of a typical index.js or main.jsx file in a React application:

```javascript 
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

// If your App is wrapped in StrictMode, you might see double renders in development mode
// ReactDOM.render(
//   <React.StrictMode>
//     <App />
//   </React.StrictMode>,
//   document.getElementById('root')
// );

// Try rendering without StrictMode
ReactDOM.render(
  <App />,
  document.getElementById('root')
);

```
