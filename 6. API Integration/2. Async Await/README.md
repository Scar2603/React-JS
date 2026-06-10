# Async/Await

## Why Async/Await?

When working with APIs, the server takes time to respond.
```
Request Sent
    ↓
Wait
    ↓
Response Received
```
JavaScript should not freeze while waiting.

Async/Await helps us write asynchronous code in a clean and readable way.

## What is a Promise?

A Promise is a placeholder for a future value.
```
const promise = fetch(URL);
```
At this moment:
```
Data is NOT available yet
```
Instead:
```
Promise = "I will give you the result later"
```
## Old Way (.then)
```
fetch(URL)
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });
```
Works correctly, but can become difficult to read in larger applications.

## Modern Way (Async/Await)
```
const response = await fetch(URL);

const data = await response.json();
```
Looks like normal synchronous code and is easier to understand.

## Rules of Async/Await
### Rule 1

`await` can only be used inside an `async` function.

✅ Correct
```
async function getUsers() {
  const response = await fetch(URL);
}
```
❌ Incorrect
```
const response = await fetch(URL);
```
### Understanding await fetch()
```
const response = await fetch(URL);
```
What happens?
```
Send Request
     ↓
Wait for Server
     ↓
Receive Response
     ↓
Store Response
```
After execution:
```
response
```
contains:
```
HTTP Response Object
```
### Why Not This?
```
const response = fetch(URL);
```
Because:
```
response
```
contains:
```
Promise
```
not the actual response.

### Understanding response.json()

API usually returns JSON.

Example:
```
{
  "name": "John"
}
```
We must convert it into a JavaScript object.
```
const data = await response.json();
```
What happens?
```
Response
     ↓
Convert JSON
     ↓
JavaScript Object
```
### Why Two Awaits?

**First Await**
```
await fetch(URL)
```
Purpose:
```
Wait for Server Response
```
**Second Await**
```
await response.json()
```
Purpose:
```
Wait for JSON Conversion
```
Complete Flow
```
const response =
  await fetch(URL);

const data =
  await response.json();
```
Flow:
```
fetch(URL)
     ↓
Promise
     ↓
await
     ↓
Response Object
     ↓
response.json()
     ↓
Promise
     ↓
await
     ↓
JavaScript Data
```
### React Example
```
useEffect(() => {

  async function getPosts() {

    const response =
      await fetch(URL);

    const data =
      await response.json();

    setPosts(data);
  }

  getPosts();

}, []);
```
### React API Flow
```
Component Mounts
       ↓
useEffect Runs
       ↓
API Request Sent
       ↓
Server Responds
       ↓
JSON Converted
       ↓
setPosts(data)
       ↓
State Updates
       ↓
Component Re-renders
       ↓
Posts Displayed
```
### Common Mistakes
**Mistake 1**

Forgetting first await.

❌
```
const response = fetch(URL);

const data = await response.json();
```
Problem:
```
response = Promise
```
✅
```
const response =
  await fetch(URL);

const data =
  await response.json();
```
**Mistake 2**

Using await outside async function.

❌
```
const response =
  await fetch(URL);
```
✅
```
async function getData() {

  const response =
    await fetch(URL);

}
```
