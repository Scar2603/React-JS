# API Calls with useEffect

Real World Analogy

Imagine Swiggy.

When you open the app:
```
Open App
 ↓
Request restaurants
 ↓
Server responds
 ↓
Show restaurants
```
The restaurants are not hardcoded.

They're fetched from a server.

## What is an API?

API = Application Programming Interface

For now think:

**API = A URL that gives data**

Example:
```
https://jsonplaceholder.typicode.com/users
```
returns:
```
[
  {
    "id": 1,
    "name": "Leanne Graham"
  },
  {
    "id": 2,
    "name": "Ervin Howell"
  }
]
```

## Why use useEffect?

Suppose we do:
```
function App() {

  fetch("https://jsonplaceholder.typicode.com/users");

  return <h1>Hello</h1>;
}
```
Bad.

Why?

Remember:
```
State Change
 ↓
Re-render
 ↓
Component runs again
```
Every render:
```
fetch(...)
```
runs again.

You might accidentally make 100 API requests instead of 1 API request

Correct Pattern
```
useEffect(() => {

  fetch(...)

}, []);
```
Why?

Because:
`[]`
means:
```
Run once after mount
```
Perfect for loading initial data.

### First API Example
```
import { useEffect } from "react";

function Users() {

  useEffect(() => {

    fetch("https://jsonplaceholder.typicode.com/users")
      .then(response => response.json())
      .then(data => {
        console.log(data);
      });

  }, []);

  return <h1>Users</h1>;
}
```
### Understanding Every Line

**fetch()**
```
fetch(url)
```
Sends HTTP request.

Think:
```
React
 ↓
"Give me users"
 ↓
Server
```
**.then()**

The server takes time.

Maybe:
```
200ms
500ms
1 second
```
We don't want React to freeze.

So JavaScript continues running.

When response arrives:
```
.then(...)
```
executes.

**response.json()**

Server sends:
```
{
  "name": "Alice"
}
```
as text.

We convert it into JavaScript object.

**data**

After conversion:
```
[
  {
    id: 1,
    name: "Leanne"
  }
]
```
becomes available.

**But There's A Problem**

We're only logging.

Not showing data.

Let's fix that.

### State + API
```
import { useEffect, useState } from "react";

function Users() {

  const [users, setUsers] = useState([]);

  useEffect(() => {

    fetch(
      "https://jsonplaceholder.typicode.com/users"
    )
      .then(response => response.json())
      .then(data => {
        setUsers(data);
      });

  }, []);

  return (
    <>
      {
        users.map(user => (
          <h3 key={user.id}>
            {user.name}
          </h3>
        ))
      }
    </>
  );
}
```
### React Flow

Mount:
```
users = []
```
Screen:
```
Nothing
```
useEffect runs:
```
Fetch data
```
Server responds:
```
10 users
```
React runs:
```
setUsers(data)
```
State changes:
```
users = [ ... ]
```
Component re-renders.

Now:
```
users.map(...)
```
renders user list.

### Visual Timeline
```
Mount
 ↓
users = []
 ↓
Render Empty UI
 ↓
useEffect Runs
 ↓
Fetch Starts
 ↓
Response Arrives
 ↓
setUsers(data)
 ↓
Re-render
 ↓
Display Users
```

### Why This Pattern Is So Important

You'll use this exact flow for:
```
Products
Orders
Customers
Movies
Books
Posts
Comments
Todos
```
Almost every application.

## Common Beginner Mistake
```
const [users, setUsers] = useState([]);

useEffect(() => {

  setUsers([...]);

}, [users]);
```
Why dangerous?
```
users changes
 ↓
effect runs
 ↓
setUsers
 ↓
users changes
 ↓
effect runs
 ↓
Infinite loop
```
## Modern Pattern: async/await

Instead of:
```
fetch(...)
 .then(...)
 .then(...)
```
Most React developers write:
```
useEffect(() => {

  async function fetchUsers() {

    const response =
      await fetch(URL);

    const data =
      await response.json();

    setUsers(data);
  }

  fetchUsers();

}, []);
```
Much cleaner.
