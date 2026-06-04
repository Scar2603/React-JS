# Props (Passing Data)

Imagine ordering pizza.
The pizza shop has ONE pizza-making machine.
Different customers send different orders.
```
Imagine ordering pizza.
The pizza shop has ONE pizza-making machine.
Different customers send different orders.
```
Same machine. Different inputs.

React components work exactly like this.  
The component is the machine  
Props are the inputs.

## What Are Props?  
Props = Properties  
Props allow one component to send data to another component.  
Without props:  
```
function Profile() {
  return <h1>Alice</h1>;
}
```
Always Alice. Not reusable.
With props:
```
function Profile(props) {
  return <h1>{props.name}</h1>;
}
```
Now reusable.  
**Example**
```
function Profile(props) {
  return <h1>{props.name}</h1>;
}

function App() {
  return (
    <>
      <Profile name="Alice" />
      <Profile name="Bob" />
      <Profile name="Charlie" />
    </>
  );
}
```
Output:
```
Alice
Bob
Charlie
```
### What Happens Internally?  
React sees:  
``` <Profile name="Alice" />```  
and executes:  
```
Profile({
  name: "Alice"
})
```

Notice:  
React passes an object.  
Inside component:
```
function Profile(props) {
props becomes:
{
  name: "Alice"
}
```
Then:``` props.name```  
returns: ``` Alice```

## Better Syntax (Destructuring)
Instead of
```
function Profile(props) {
  return <h1>{props.name}</h1>;
}
```
Use
```
function Profile({ name }) {
  return <h1>{name}</h1>;
}
```
Much cleaner.   

## Multiple Props
```
function Profile({ name, role }) {
  return (
    <>
      <h2>{name}</h2>
      <p>{role}</p>
    </>
  );
}
```
Usage:  
```
<Profile
  name="Alice"
  role="Developer"
/>
```
### Important Rule  
Props are Read Only.  
❌ ``` props.name = "Bob"; ``` Never do this.

React follows:
```
Parent
   |
   v
Child
```
Data flows downward only.


### Common Mistakes
**Mistake 1**  
Forgetting {}

❌
```
<h1>props.name</h1>
```

✅
```
<h1>{props.name}</h1>
```
**Mistake 2**  
Trying to modify props

❌
```
props.name = "New Name";
```
**Mistake 3**
Wrong destructuring

❌
```
function Profile(name) {}
```

✅
```
function Profile({ name }) {}
```




# State
Imagine a light switch.
The light can be: ```on``` or ```off```  
When you press the switch, the value changes, and the room changes visually.  
React state is like that.  
State is data that can change and cause the UI to update.

## What is State?
State is a component’s private memory.    
Props come from outside.  
State lives inside the component.     
```
Props = data received from parent
State = data owned by component
```
### Why State Exists  
Without state, React UI is static.  
Example:
```
function Counter() {
  let count = 0;

  return <h1>{count}</h1>;
}
```
This shows: ```0```   
But if `count` changes, React will not automatically update the screen.  
So React gives us `useState`.

### Syntax
```
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return <h1>{count}</h1>;
}
```
Line by line:  
`import { useState } from "react";` We bring React’s state tool into the file.  
`const [count, setCount] = useState(0);` This creates state.  
`count` current value.  
`setCount` Function used to update value.  
`useState(0)` Initial value is 0.  

**Example: Counter**
```
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Count: {count}</h2>

      <button onClick={() => setCount(count + 1)}>
        Increase
      </button>
    </div>
  );
}

export default Counter;
```

### What happens in browser?

Initial render:

`Count: 0`

You click button.

React runs:

`setCount(count + 1)`

State becomes:

`1`

React re-renders component.

Browser shows:

`Count: 1`

### Internal Behavior

When state changes:
```
User clicks button
        ↓
setCount is called
        ↓
React stores new state value
        ↓
React re-renders component
        ↓
JSX is recalculated
        ↓
React compares old UI and new UI
        ↓
Browser DOM is updated only where needed
```

## Common Mistake  
❌ Do not update state like this:  
`count = count + 1;`  
Why?

Because React does not know you changed it.

✅ Use setter:  
``setCount(count + 1);``  

## Best Practice

When new state depends on old state, use callback form:

``setCount(prevCount => prevCount + 1);``

Better than:

``setCount(count + 1);``

Because React state updates can be batched.













