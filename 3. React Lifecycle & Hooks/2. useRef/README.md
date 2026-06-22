# useRef

Imagine your room.

You have:

### Whiteboard
```
Write value
 ↓
Everyone sees change
``` 
This is like:
```
useState
```
When it changes:
```
UI updates
```
## Sticky Note In Your Pocket
```
Write value
 ↓
Nobody sees change
```
This is like:
```
useRef
```
When it changes:
```
UI does NOT update
```
First Big Difference
## useState
```
const [count, setCount] = useState(0);
```
When:
```
setCount(1);
```
React does:
```
State changes
 ↓
Re-render
 ↓
UI updates
```
## useRef
```
const countRef = useRef(0);
```
When:
```
countRef.current = 1;
```
React does:
```
Value changes
 ↓
NO re-render
```
This is the most important thing to memorize.

## What Does useRef Return?
```
const countRef = useRef(0);
```
Returns:
```
{
  current: 0
}
```
Think:
```
countRef.current
```
as:
```
the actual stored value
```
### Example
```
import { useRef } from "react";

function App() {

  const countRef = useRef(0);

  function increase() {

    countRef.current++;

    console.log(countRef.current);
  }

  return (
    <button onClick={increase}>
      Increase
    </button>
  );
}
```
Click 1:
```
1
```
Click 2:
```
2
```
Click 3:
```
3
```
Console updates.

UI doesn't.

Why?

Because:
```
Ref changed
 ↓
State did not change
 ↓
No re-render
```
## Why Does useRef Exist?

Two main reasons.

## Use Case 1: Access DOM Elements

Suppose:
```
<input />
```
When page loads we want:
```
Cursor automatically inside input
```
HTML way:
```
document.getElementById(...)
```
React way:
```
useRef
```
### Example:
```
import { useRef, useEffect } from "react";

function App() {

  const inputRef = useRef(null);

  useEffect(() => {

    inputRef.current.focus();

  }, []);

  return (
    <input ref={inputRef} />
  );
}
```
### What Happens?

Mount:
```
Input created
```
React stores reference:
```
inputRef.current
```
becomes:
```
<input />
```
actual DOM element.

useEffect:
```
inputRef.current.focus();
```
runs.

Cursor appears automatically.

### Visual Flow
```
Input Rendered
 ↓
Ref Attached
 ↓
inputRef.current
 ↓
Actual DOM Node
 ↓
focus()
```
## Use Case 2: Persist Values Between Renders

Suppose:
```
const renderCount = useRef(0);
```
Every render:
```
renderCount.current++;
```
This value survives re-renders.

Unlike:
```
let count = 0;
```
which resets every render.

## Comparison
### Normal Variable
```
function App() {

  let count = 0;

  count++;

  console.log(count);
}
```
Every render:
```
1
```
because variable resets.

### Ref
```
const countRef = useRef(0);

countRef.current++;
```
Every render:
```
1
2
3
4
```
because ref persists.

## State vs Ref

| Feature                  | useState          | useRef      |
|--------------------------|-------------------|-------------|
| Persists Between Renders | ✅                | ✅          |
| Causes Re-render         | ✅                | ❌          |
| Stores DOM Element       | ❌                | ✅          |
| Stores UI Data           | ✅                | Usually No  |
| Mutable                  | Through setter    | Directly    |

## Common Mistake

Beginners try:
```
const countRef = useRef(0);

return (
  <h1>{countRef.current}</h1>
);
```
Then:
```
countRef.current++;
```
and expect UI update.

❌ Doesn't happen.

Because:
```
Ref changed
 ↓
No re-render
```
