# Events

Now we'll connect:
```
User Action
    ↓
Event
    ↓
State Change
    ↓
Re-render
    ↓
Updated UI
```
This is React's core loop.

Imagine a doorbell.
```
Visitor presses bell
        ↓
Bell rings
        ↓
You open door
```
The button press is an event.

React works the same way.

## What is an Event?

An event is something the user does.

Examples:
```
Click
Typing
Hover
Submit
Scroll
Focus
```
React listens for these events.

## Why Events Exist

Without events:
```
UI can display data
```
With events:
```
UI can interact with users
```
Interactive applications become possible.

## Syntax

HTML:
```
<button onclick="doSomething()">
```
React:
```
<button onClick={doSomething}>
```
Notice:
```
onClick
```
Capital C.

## Example
```
function App() {

  function sayHello() {
    alert("Hello");
  }

  return (
    <button onClick={sayHello}>
      Click Me
    </button>
  );
}
```
## What Happens Internally?

Browser:
```
User clicks button
```
↓
```
React event system catches click
```
↓

React calls:
```
sayHello()
```
↓

Alert appears

## Event + State Together

This is where React becomes powerful.
```
import { useState } from "react";

function Counter() {

  const [count, setCount] = useState(0);

  return (
    <>
      <h1>{count}</h1>

      <button
        onClick={() => setCount(prev => prev + 1)}
      >
        Increase
      </button>
    </>
  );
}
```
Flow:
```
Click
 ↓
onClick fires
 ↓
setCount
 ↓
State changes
 ↓
Re-render
 ↓
New UI
```

## Common Mistake #1

❌
```
<button onClick={sayHello()}>
```
This executes immediately during render.

Why?

Because:
```
sayHello()
```
means:
```
Run now
```
✅
```
<button onClick={sayHello}>
```
means:

Run later when clicked

## Common Mistake #2

Passing arguments incorrectly.

❌
```
<button onClick={increment(5)}>
```
Runs immediately.

✅
```
<button onClick={() => increment(5)}>
```
Creates function.

Runs on click.

## Event Object

React gives information about the event.

Example:
```
function handleClick(event) {
  console.log(event);
}
<button onClick={handleClick}>
```
The event object contains:
```
Which button
Mouse position
Target element
Keyboard info
...
```


