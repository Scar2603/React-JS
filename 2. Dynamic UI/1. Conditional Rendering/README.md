# Conditional Rendering

Imagine Netflix.

When you are logged in:
```
Welcome {user name}
My Profile
Logout
```

When you are not logged in:
```
Login
Register
```
Same page. Different UI.

React does this using: **Conditional Rendering**

Showing different UI depending on a condition.

Example:
```
If logged in
   Show Dashboard

Else
   Show Login Page
```
## Why Do We Need It?

Without conditional rendering:
```
UI always looks the same
```
With conditional rendering:
```
UI adapts to data
```
### Example 1: if statement
```
function App() {

  const isLoggedIn = true;

  if (isLoggedIn) {
    return <h1>Welcome Back</h1>;
  }

  return <h1>Please Login</h1>;
}
```
**What Happens Internally?**

React runs:
```
if (isLoggedIn)
```
If true:
```
<h1>Welcome Back</h1>
```
is returned.

Otherwise:
```
<h1>Please Login</h1>
```
is returned.

React only renders what is returned.

### Example 2: Ternary Operator

Most common in React.

Syntax:
```
condition ? value1 : value2
```
Example:
```
function App() {

  const isLoggedIn = true;

  return (
    <div>
      {
        isLoggedIn
          ? <h1>Dashboard</h1>
          : <h1>Login</h1>
      }
    </div>
  );
}
```
Think:
```
Condition ?
    TRUE UI
:
    FALSE UI
```
### Example 3: && Operator

When you only need to show something.
```
const isAdmin = true;

return (
  <>
    {isAdmin && <button>Delete User</button>}
  </>
);
```
Meaning:
```
If admin
   Show button

Otherwise
   Show nothing
```
**Internally**

JavaScript evaluates:
```
true && "Hello"
```
Result:
```
"Hello"
false && "Hello"
```
Result:
```
false
```
React ignores false.

Nothing renders.

### Real Example
```
function UserStatus() {

  const [isOnline, setIsOnline] =
    useState(true);

  return (
    <>
      {
        isOnline
          ? <h2>🟢 Online</h2>
          : <h2>🔴 Offline</h2>
      }

      <button
        onClick={() =>
          setIsOnline(prev => !prev)
        }
      >
        Toggle
      </button>
    </>
  );
}
```

## New Operator: !

You haven't seen this yet.
```
!true
```
becomes:
```
false
```
```
!false
```
becomes:
```
true
```
Used for toggling.
```
setIsOnline(prev => !prev)
```
If:
```
prev = true
```
new value:
```
false
```
If:
```
prev = false
```
new value:
```
true
```
### Common Mistakes
**Mistake 1**

Using if directly inside JSX

❌
```
return (
  <div>
    if(isLoggedIn) {
      <h1>Hello</h1>
    }
  </div>
);
```
Not allowed.

**Mistake 2**

Forgetting false case

Sometimes:
```
condition ? <A />
```
❌ Invalid

Need:
```
condition ? <A /> : <B />
```
