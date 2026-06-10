# Lifecycle + useEffect

Imagine a person.

They have phases:
```
Born
 ↓
Live
 ↓
Update
 ↓
Die
```
React components have similar phases.
```
Mount
 ↓
Update
 ↓
Unmount
```

## React Lifecycle
### 1. Mount

Component appears for the first time.
```
<App />
```
gets rendered.

Example:
```
function Profile() {
  return <h1>Profile</h1>;
}
```
First time it appears:
```
MOUNT
```
### 2. Update

Something changes.

Usually:
```
State
or
Props
```
change.

Example:
```
setCount(5);
```
React re-renders.
```
UPDATE
```
### 3. Unmount

Component disappears.

Example:
```
{
  showProfile && <Profile />
}
```
When:
```
showProfile = false
```
Profile is removed.
```
UNMOUNT
```

## Visual Timeline
```
Component Created
       ↓
     MOUNT
       ↓
User Clicks
       ↓
     UPDATE
       ↓
User Clicks Again
       ↓
     UPDATE
       ↓
Component Removed
       ↓
    UNMOUNT
```
## Why useEffect Exists

Now the big question.

Suppose we want:
```
Call API
Start Timer
Add Event Listener
Save Data
```
Should we do:
```
function App() {

  fetch("/users");

  return <h1>Hello</h1>;
}
```
?

❌ Dangerous.

Why?

Because component runs on every render.

Remember:
```
Render
 ↓
Component Function Runs
```
So:
```
fetch("/users");
```
would run again and again.

React created:

## **useEffect**

to handle side effects safely.

What is a Side Effect?

A side effect is something that affects the outside world.

Examples:

- API calls
- Timers
- Local Storage
- Event Listeners
- Analytics

Not side effects:

- JSX
- Rendering UI
- Calculating values

### First useEffect Example
```
import { useEffect } from "react";

function App() {

  useEffect(() => {
    console.log("Mounted");
  }, []);

  return <h1>Hello</h1>;
}
```
### New Syntax
```
useEffect(
  callback,
  dependencies
)
```
Part 1
```
() => {
  console.log("Mounted");
}
```
Code to run.

Part 2
```
[]
```
Dependency Array.

Controls when effect runs.

## Empty Dependency Array
```
useEffect(() => {
  console.log("Mounted");
}, []);
```
Runs:
```
ONLY after first render
```
Timeline:
```
Mount
 ↓
Effect Runs
```
Even if component re-renders:
```
Update
 ↓
Effect does NOT run again
```

## The Most Important useEffect Concept

**Before learning dependency arrays, you must understand:**
```
useEffect(
  callback,
  dependencies
)
```
The dependency array controls WHEN the effect runs.

There are only 3 patterns you must memorize.

### Pattern 1: Empty Dependency Array
```
useEffect(() => {
  console.log("Mounted");
}, []);
```
Runs: **Only once after mount**

Timeline:
```
Mount
 ↓
Effect runs

Update
 ↓
No effect

Update
 ↓
No effect
```
### Pattern 2: Dependency Array With Value
```
useEffect(() => {
  console.log("Count changed");
}, [count]);
```
Runs:
```
Mount
AND
Whenever count changes
```
Timeline:
```
Mount
 ↓
Effect

count changes
 ↓
Effect

count changes again
 ↓
Effect
```
### Pattern 3: No Dependency Array
```
useEffect(() => {
  console.log("Every render");
});
```
Runs: **Every render**

Timeline:
```
Mount
 ↓
Effect

Update
 ↓
Effect

Update
 ↓
Effect
```
### Visual Comparison

### **[]**
```
useEffect(() => {}, []);
```
```
Mount ✅
Update ❌
Update ❌
```
### **[count]**
```
useEffect(() => {}, [count]);
```
```
Mount ✅
Count change ✅
Count change ✅
Other state change ❌
```
### **No array**
```
useEffect(() => {});
```
```
Mount ✅
Update ✅
Update ✅
Update ✅
```

### Practical Example
```
import { useState, useEffect } from "react";

function Counter() {

  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Count changed:", count);
  }, [count]);

  return (
    <>
      <h1>{count}</h1>

      <button
        onClick={() =>
          setCount(prev => prev + 1)
        }
      >
        Increase
      </button>
    </>
  );
}
```
Initial:
```
Count = 0
```
Console:
```
Count changed: 0
```
Click:
```
Count = 1
```
Console:
```
Count changed: 1
```
Click:
```
Count = 2
```
Console:
```
Count changed: 2
```

## Common Beginner Mistake
```
useEffect(() => {
  setCount(count + 1);
}, [count]);
```
Danger!

Why?
```
Count changes
 ↓
Effect runs
 ↓
Count changes again
 ↓
Effect runs
 ↓
Count changes again
 ↓
Infinite loop
```

## Dependency Arrays Deep Dive

**Question**:

If we have:
```
const [count, setCount] = useState(0);
const [name, setName] = useState("Sanskar");

useEffect(() => {
  console.log("Effect ran");
}, [count]);
```
What happens when:

### A) count changes?
### B) name changes?

Will the effect run in both cases or only one?

### Answer

Given:
```
const [count, setCount] = useState(0);
const [name, setName] = useState("Sanskar");

useEffect(() => {
  console.log("Effect ran");
}, [count]);
```
**A) count changes**

Example:
```
setCount(1);
```
What happens?
```
State changes
 ↓
Component re-renders
 ↓
React checks dependency array
 ↓
Old count = 0
New count = 1
 ↓
Dependency changed
 ↓
Effect runs
```
Output:
```
Effect ran
```

**B) name changes**

Example:
```
setName("John");
```
What happens?
```
State changes
 ↓
Component re-renders
```
Notice something important:

The component DOES re-render.

Many beginners think:
```
Dependency didn't change
 ↓
No re-render
```
❌ Wrong

The component still re-renders.

React must run the component again because state changed.

Then React checks:
```
[count]
```
Old:
```
count = 0
```
New:
```
count = 0
```
No change.

Therefore:
```
Effect does NOT run
```
### Super Important Distinction

Many developers confuse these:
```
Re-render
```
and
```
useEffect execution
```
They are NOT the same thing.

Example:
```
const [count, setCount] = useState(0);
const [name, setName] = useState("");
useEffect(() => {
  console.log("Effect");
}, [count]);
```
User changes name:
```
setName("Alice");
```
Result:
```
Component Re-renders ✅
Effect Runs ❌
```
**Golden Rule**
```
State change
 ↓
Re-render always happens
```
Then:
```
React checks dependency array
 ↓
If dependency changed
    Run effect
Else
    Skip effect
```
### Visual Timeline

Initial:
```
count = 0
name = ""
```
Effect runs.

User changes name:
```
name = "Alice"
```
```
Re-render ✅
Effect ❌
```
User changes count:
```
count = 1
```
```
Re-render ✅
Effect ✅
```
