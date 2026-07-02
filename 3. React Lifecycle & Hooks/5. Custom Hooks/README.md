# Custom Hooks

## Real-world Analogy

Imagine you're a chef.

You know how to make:

  - Dough
  - Sauce
  - Cheese
  - Vegetables

Every time someone orders pizza, you repeat all the steps.

After making 100 pizzas, you think:

>"Why don't I create one recipe called makePizza()?"

Now instead of writing:
```
Make dough
Add sauce
Add cheese
Bake
```
every time,

you simply call:
```
makePizza()
```
That's exactly what a custom hook is.

## What is a Custom Hook?

A custom hook is:

>A JavaScript function that uses one or more React hooks to encapsulate reusable logic.

Notice the wording.

It is not a special React object.

It is just a function.

### Why Do We Need It?

Suppose you have three components.
```
Login Page
Profile Page
Dashboard
```
All of them need:
```
Fetch Users
Loading State
Error State
```
Without a custom hook:
```
// Login
useState(...)
useEffect(...)

// Profile
useState(...)
useEffect(...)

// Dashboard
useState(...)
useEffect(...)
```
You copy the same code three times.

**Bad**.

With a custom hook:
```
const { users, loading, error } = useUsers();
```
**Done**.

### Rule #1

Custom hook names must start with:

use

Examples:
```
useFetch
useCounter
useTheme
useToggle
useLocalStorage
```
React uses this naming convention to identify hooks.

### First Custom Hook

Let's build one you've already written manually several times.

### **useCounter**

**useCounter.js**
```
import { useState } from "react";

function useCounter(initialValue = 0) {

  const [count, setCount] = useState(initialValue);

  function increment() {
    setCount(prev => prev + 1);
  }

  function decrement() {
    setCount(prev => prev - 1);
  }

  function reset() {
    setCount(initialValue);
  }

  return {
    count,
    increment,
    decrement,
    reset
  };
}

export default useCounter;
```

## Let's Understand Every Line
### Function
```
function useCounter(initialValue = 0)
```
Just a normal JavaScript function.

The only difference is:

Starts with:
```
use
```
### useState
```
const [count, setCount] = useState(initialValue);
```
Exactly the same `useState` you've already learned.

### increment
```
function increment() {
  setCount(prev => prev + 1);
}
```
Nothing new.

### Return
```
return {
  count,
  increment,
  decrement,
  reset
};
```
Instead of returning one value,

we return an object.

### Using It
```
import useCounter from "./useCounter";

function App() {

  const {
    count,
    increment,
    decrement,
    reset
  } = useCounter(10);

  return (
    <>
      <h1>{count}</h1>

      <button onClick={increment}>
        +
      </button>

      <button onClick={decrement}>
        -
      </button>

      <button onClick={reset}>
        Reset
      </button>
    </>
  );
}
```

## What Happens Internally?

You write:
```
const {
  count,
  increment
} = useCounter();
```
React executes:
```
useCounter()
```
Inside it:
```
useState(...)
```
creates state.

Then returns:
```
{
  count,
  increment,
  decrement,
  reset
}
```
Your component receives that object.

### Important Question

Does every component share the same counter?

Example:
```
function App() {

  return (
    <>
      <Counter />
      <Counter />
    </>
  );
}
```
Each Counter uses:
```
useCounter()
```
**Question**:

Do they share state?

Answer:

❌ No.

Each call gets its own independent state.

This is very important.

## Real-world Custom Hook

The hook you wrote earlier manually:
```
const [posts, setPosts] = useState([]);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

useEffect(() => {
  ...
}, []);
```
could become:
```
const {
  data,
  loading,
  error
} = useFetch(URL);
```
Beautiful.

## Mental Model

Without custom hook:
```
Component
 ├── useState
 ├── useEffect
 ├── useRef
 └── useMemo
```
With custom hook:
```
Component
      │
      ▼
 useFetch()
      │
      ▼
 Internally:
   useState
   useEffect
   useRef
```
The component becomes much cleaner.

## Best Practices

✅ Custom hooks should encapsulate logic, not UI.

Good:
```
useCounter()
useFetch()
useTheme()
```
Bad:
```
useButton()
```
if it returns JSX.

Hooks return **data and functions**, not components.
