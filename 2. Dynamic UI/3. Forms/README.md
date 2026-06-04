# Forms

Everything you've learned so far comes together here:
- JSX
- Components
- Props
- State
- Events
- Conditional Rendering

Forms are where React starts feeling like a real application.

Imagine a paper form:
```
Name: _______
Email: _______
```
User types.

Data changes.

The form remembers the latest values.

React forms work exactly the same way.

## What is a Form?

A form is UI that collects user input.

Examples:
```
Login Form
Search Box
Registration Form
Feedback Form
Todo Input
```
## Controlled Components

This is the React way.

The input's value is controlled by React state.
```
Input
 ↓
State
 ↓
UI
```
### Example
```
import { useState } from "react";

function NameForm() {

  const [name, setName] = useState("");

  return (
    <>
      <input
        value={name}
        onChange={(event) =>
          setName(event.target.value)
        }
      />

      <h2>Hello {name}</h2>
    </>
  );
}
```
## What Happens Internally?

Initial:
```
name = ""
```
Input:
```
empty
```
User types:
```
A
```
Browser fires:
```
onChange
```
React receives:
```
event.target.value
```
which is:
```
"A"
```
React runs:
```
setName("A")
```
State changes:
```
name = "A"
```
React re-renders:
```
Hello A
```
appears.

## Important New Thing
### Event Object

You saw this earlier.

Now we use it.
```
function handleChange(event) {
  console.log(event.target.value);
}
```
Suppose user types:
```
Hello
```
then:
```
event.target.value
```
is:
```
"Hello"
```
## Why Use value={name}?

This is crucial.
```
<input
  value={name}
  onChange={handleChange}
/>
```
This makes React the single source of truth.

The displayed value always comes from state.
