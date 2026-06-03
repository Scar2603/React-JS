# React Environment + JSX + Components

These three belong together.
Without them nothing else makes sense.

## What is React?
Imagine building a house.
A house contains:
- Door
- Window
- Kitchen
- Bedroom

Instead of building the entire house as one giant object, you build small reusable pieces.
React does exactly this.
A webpage becomes:
```
Navbar
Sidebar
Profile
Footer
Button
Card
```

Each piece is called a: **Component**

### What is React?
React is a JavaScript library for building User Interfaces (UI). Created by Facebook.

## Why React Exists
Before React:
Imagine changing a profile name.
JavaScript had to:
```
document.getElementById(...)
document.createElement(...)
document.appendChild(...)
```
Lots of manual work.
React says:
```
<h1>John</h1>
```
React handles DOM updates automatically.

## Environment Setup
Modern React uses: **Vite**
Why?
Because it's:
- Fast
- Lightweight
- Modern

Create project:
```
npm create vite@latest my-app
```
Move into project:
```
cd my-app
```
Install packages:
```
npm install
```
Run:
```
npm run dev
```
## Project Structure
### Typical Vite project:
```
my-app
│
├── node_modules
├── public
├── src
│   ├── App.jsx
│   ├── main.jsx
│
├── package.json
└── vite.config.js
```
Most of your work happens inside: ``` src```

## What is JSX?
Imagine writing: ```<h1>Hello</h1>``` inside JavaScript. That's JSX.

### Definition 
JSX means: **JavaScript XML**
Example:
```
const element = <h1>Hello React</h1>;
```
Looks like HTML. But it is NOT HTML.

### Internally What Happens?
You write: ```<h1>Hello</h1>```
React converts it into:
```
React.createElement(
  "h1",
  null,
  "Hello"
);
```
Browser never sees JSX. Browser sees JavaScript objects.

## JSX Rules
### Rule 1
Return one parent element:

❌
```
return (
  <h1>Hello</h1>
  <p>World</p>
);
```
✅
```
return (
  <div>
    <h1>Hello</h1>
    <p>World</p>
  </div>
);
```
### Rule 2
Use {} to inject JavaScript.
```
const name = "John";
<h1>{name}</h1>
```
Output: ```John```
### Rule 3
Use className NOT class:

❌
```
<div class="box">
```
✅
```
<div className="box">
```
### Example
```
function App() {
  const name = "John";

  return (
    <h1>Hello {name}</h1>
  );
}
```
Line-by-line: 
``` function App()``` Creates a component.
```const name = "John";``` Stores value.
```<h1>Hello {name}</h1>``` JSX
React inserts value.
Result:
```
<h1>Hello John</h1>
```

## What is a Component?
Think LEGO blocks. You create one block. Reuse it everywhere.
React components are UI LEGO blocks.

### Component Definition
A component is a JavaScript function that returns JSX.
**Example:**
```
function Welcome() {
  return <h1>Welcome</h1>;
}
```
### Using Components
```
function Welcome() {
  return <h1>Welcome</h1>;
}

function App() {
  return (
    <div>
      <Welcome />
      <Welcome />
      <Welcome />
    </div>
  );
}
```
**Output:**
```
Welcome
Welcome
Welcome
```

### What Happens Internally?
React sees: ``` <Welcome />```
and executes: ``` Welcome()```

The returned JSX becomes UI.

### Common Beginner Mistakes
**Mistake 1**
Lowercase component name

❌
``` function welcome() {} ```

React treats it like HTML.

✅
``` function Welcome() {}  ```

**Mistake 2**
Multiple root elements

❌
```
return (
  <h1>Hello</h1>
  <p>World</p>
);
```
**Mistake 3**
Forgetting {}

❌ ``` <h1>name</h1> ```
Shows: ``` name ```

✅ ``` <h1>{name}</h1>```
Shows: ``` John```
