# Context API

Before I learn it, let's start with the problem it solves.

Imagine:
```
App
 ↓
Dashboard
 ↓
Sidebar
 ↓
Profile
 ↓
Avatar
```
User data exists in:
```
App
```
```
const user = {
  name: "Shankar"
};
```
But Avatar needs it.

Without Context:
```
App
 ↓ props
Dashboard
 ↓ props
Sidebar
 ↓ props
Profile
 ↓ props
Avatar
```
Even though:
```
Dashboard doesn't use user
Sidebar doesn't use user
Profile doesn't use user
```
they must pass it down.

This problem is called:

## Prop Drilling

Suppose:
```
<App user={user} />
```
and the component tree is:
```
App
 ↓
Dashboard
 ↓
Sidebar
 ↓
Profile
 ↓
Avatar
```
If Avatar needs the user, how would you pass it using only props?

```
App
 ↓ user
Dashboard
 ↓ user
Sidebar
 ↓ user
Profile
 ↓ user
Avatar
```

Notice something strange.

Who actually needs the user?
```
Avatar ✅
```
Who doesn't?
```
Dashboard ❌
Sidebar ❌
Profile ❌
```
Yet they are forced to receive and pass the prop.

### Real World Analogy

Imagine a CEO wants to send a message to an intern.

Without Context:
```
CEO
 ↓
Manager
 ↓
Team Lead
 ↓
Senior Dev
 ↓
Intern
```
Even though:
```
Manager doesn't need message
Team Lead doesn't need message
Senior Dev doesn't need message
```
they must forward it.

**Annoying**.

### This Is Prop Drilling
```
function App() {

  const user = "Shankar";

  return (
    <Dashboard user={user} />
  );
}
```
**Dashboard**:
```
function Dashboard({ user }) {

  return (
    <Sidebar user={user} />
  );
}
```
**Sidebar**:
```
function Sidebar({ user }) {

  return (
    <Profile user={user} />
  );
}
```
**Profile**:
```
function Profile({ user }) {

  return (
    <Avatar user={user} />
  );
}
```
**Avatar**:
```
function Avatar({ user }) {

  return (
    <h1>{user}</h1>
  );
}
```
Look how ridiculous this becomes.
```
Dashboard
  doesn't use user

Sidebar
  doesn't use user

Profile
  doesn't use user
```
But all must pass it.

## Context API Solution

React says:
```
Instead of passing through every floor,
put the data in a central room.
```
Then any component can access it directly.

**Without Context**:
```
App
 ↓
Dashboard
 ↓
Sidebar
 ↓
Profile
 ↓
Avatar
```
**With Context**:
```
App
 │
 │ Context
 │
 ▼
Avatar
```
Avatar can directly access the data.

No prop drilling.

## Context API Mental Model

Think of Context as:
```
Global Data Shelf
```
Any component can:
```
Read from shelf
```
without props.

## Three Steps of Context

**You only need to memorize 3 steps**.

### Step 1

Create Context
```
import { createContext } from "react";

const UserContext =
  createContext();
```
Think:
```
Create global shelf
```
### Step 2

Provide Data
```
<UserContext.Provider
  value="Shankar"
>
  <App />
</UserContext.Provider>
```
Think:
```
Put data on shelf
```
## #Step 3

Consume Data
```
import { useContext } from "react";

const user =
  useContext(UserContext);
```
Think:
```
Take data from shelf
```
## Complete Example
### UserContext.js
```
import { createContext } from "react";

export const UserContext =
  createContext();
```
### App.jsx
```
import { UserContext }
from "./UserContext";

function App() {

  return (

    <UserContext.Provider
      value="Shankar"
    >

      <Dashboard />

    </UserContext.Provider>

  );
}
```
### Avatar.jsx
```
import { useContext }
from "react";

import { UserContext }
from "./UserContext";

function Avatar() {

  const user =
    useContext(UserContext);

  return (
    <h1>{user}</h1>
  );
}
```
### Output:
```
Shankar
```
## What Happens Internally?

React stores:
```
UserContext
   ↓
"Shankar"
```
Avatar asks:
```
useContext(UserContext)
```
React responds:
```
"Shankar"
```
No props needed.

### Visual Flow

**Without Context**:
```
App
 ↓
Dashboard
 ↓
Sidebar
 ↓
Profile
 ↓
Avatar
```
**With Context**:
```
UserContext
      │
      │
      ▼
Avatar
```
