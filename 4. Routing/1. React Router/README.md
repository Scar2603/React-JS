# React Router

So far, every app we've built has been:
```
One Page
```
Real apps:
```
/          Home
/products  Products
/about     About
/profile   Profile
```
Need multiple pages.

### Real World Analogy

Imagine a mall.

Different shops:
```
Home
 ↓
Electronics
 ↓
Clothing
 ↓
Food Court
```
You walk between them.

The mall stays the same.

Only the content changes.

React Router does exactly that.

## What is React Router?

React Router lets React create:
```
Single Page Applications (SPA)
```
with multiple pages.

## What is SPA?

### Traditional website:
```
Click Link
 ↓
Browser requests new HTML
 ↓
Entire page reloads
```
### React SPA:
```
Click Link
 ↓
React changes component
 ↓
No page reload
```
Much faster.

## Installation
```
npm install react-router-dom
```
## Basic Setup
### main.jsx
```
import ReactDOM from "react-dom/client";
import App from "./App";
import {BrowserRouter} from "react-router-dom";

ReactDOM.createRoot(document.getElementById("root")).render(

  <BrowserRouter>
    <App />
  </BrowserRouter>

);
```

## Create Pages

### Home.jsx
```
function Home() {
  return <h1>Home Page</h1>;
}

export default Home;
```
### About.jsx
```
function About() {
  return <h1>About Page</h1>;
}

export default About;
```

## Create Routes

### App.jsx
```
import {Routes,Route} from "react-router-dom";

import Home from "./Home";
import About from "./About";

function App() {

  return (

    <Routes>

      <Route
        path="/"
        element={<Home />}
      />

      <Route
        path="/about"
        element={<About />}
      />

    </Routes>

  );
}
```
## What Happens Internally?

### Browser URL:
```
/
```
### React Router:
```
Matches "/"
 ↓
Render Home
```
### Browser URL:
```
/about
```
### React Router:
```
Matches "/about"
 ↓
Render About
```

## Navigation

**Wrong:**
```
<a href="/about">
  About
</a>
```
This reloads the page.

React Way:
```
import { Link } from "react-router-dom";

<Link to="/about">
  About
</Link>
```
Why?
```
Link
 ↓
Client-side navigation
 ↓
No full page reload
```
**Example**
```
<Link to="/">
  Home
</Link>

<Link to="/about">
  About
</Link>
```
