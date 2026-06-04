# Lists & Keys

Now we stop manually writing components.

Instead of:
```
<UserCard />
<UserCard />
<UserCard />
<UserCard />
<UserCard />
```
we'll generate them automatically from data.

This is one of the most common things you'll do in React.

<hr />

Imagine a restaurant.

Customers:
```
Alice
Bob
Charlie
David
```
The waiter doesn't manually build four tables.

Instead:
```
For each customer
    Create a table
```
React does the same thing.

## What Are Lists?

Lists allow React to render UI from arrays.

Example:
```
const users = [
  "Alice",
  "Bob",
  "Charlie"
];
```
Instead of writing:
```
<h1>Alice</h1>
<h1>Bob</h1>
<h1>Charlie</h1>
```
we generate them.

## The map() Function

React uses JavaScript's:
```
map()
```
method.

**What is map()?**

Given:
```
const numbers = [1, 2, 3];
numbers.map(number => number * 2);
```
returns:
```
[2, 4, 6]
```
Think:
```
Take every item
 ↓
Transform it
 ↓
Return new array
```
### React Example
```
function App() {

  const users = [
    "Alice",
    "Bob",
    "Charlie"
  ];

  return (
    <>
      {
        users.map(user => (
          <h2>{user}</h2>
        ))
      }
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
### Internal Flow

React sees:
```
[
  "Alice",
  "Bob",
  "Charlie"
]
```
map produces:
```
[
  <h2>Alice</h2>,
  <h2>Bob</h2>,
  <h2>Charlie</h2>
]
```
React renders them.

### Why Keys Exist

Now the important part.

Suppose:
```
Alice
Bob
Charlie
```
renders.

Then later:
```
Alice
Charlie
```
Bob was removed.

How does React know which item disappeared?

Answer: **Keys**

**Example**
```
{
  users.map(user => (
    <h2 key={user}>
      {user}
    </h2>
  ))
}
```
Key is a unique identifier.

**Better Example**

Usually data comes like:
```
const users = [
  {
    id: 1,
    name: "Alice"
  },
  {
    id: 2,
    name: "Bob"
  },
  {
    id: 3,
    name: "Charlie"
  }
];
```
Render:
```
{
  users.map(user => (
    <h2 key={user.id}>
      {user.name}
    </h2>
  ))
}
```

### Why Not Use Index?

Many beginners write:
```
{
  users.map((user, index) => (
    <h2 key={index}>
      {user.name}
    </h2>
  ))
}
```
React allows it.

But avoid it.
<hr/>
Imagine:

Initial:
```
0 Alice
1 Bob
2 Charlie
```
Remove Alice:
```
0 Bob
1 Charlie
```
Indexes changed.

React gets confused about which item moved.

Can cause:

- Wrong state
- Wrong animations
- Performance issues

Preferred:
```
key={user.id}
```  
because IDs don't change.

### Common Mistakes
**Mistake 1**

No key
```
users.map(user => (
  <h2>{user.name}</h2>
))
```
React warning.

**Mistake 2**

Using random key
```
key={Math.random()}
```
Terrible.

Changes every render.

**Mistake 3**

Using index unnecessarily
```
key={index}
```
Only use if list never changes.
