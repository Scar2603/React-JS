# useCallback

## Real-world Analogy

Imagine you're a teacher.

Every morning you print the same attendance sheet.

Monday:
```
Attendance Sheet #1
```
Tuesday:
```
Attendance Sheet #2
```
Wednesday:
```
Attendance Sheet #3
```
Even though nothing changed.

Waste of paper.

A smarter approach:
```
Create one attendance sheet
↓
Reuse it every day
```
That's exactly what useCallback does for functions.

## First, A Very Important Fact

Suppose:
```
function App() {

  function greet() {
    console.log("Hello");
  }

  return <button onClick={greet}>Click</button>;
}
```
Many beginners think:

The greet function exists only once.
```
❌ Wrong.
```
Remember what you've already learned:
```
When a component re-renders, the entire component function executes again.
```
That means:
```
function App() {
```
runs again.

And therefore:
```
function greet() {}
```
is created again.

### Mental Model

Render #1
```
App()
 │
 └── greet()  ← Function A
```
Render #2
```
App()
 │
 └── greet()  ← Function B
```
Render #3
```
App()
 │
 └── greet()  ← Function C
```
Different function objects.

Even though the code looks identical.

## Why Is This A Problem?

Let's build two components.
```
function Child({ onClick }) {
  console.log("Child Rendered");

  return (
    <button onClick={onClick}>
      Child Button
    </button>
  );
}
```
**Parent**:
```
function App() {

  const [count, setCount] = useState(0);

  function handleClick() {
    console.log("Clicked");
  }

  return (
    <>
      <button
        onClick={() => setCount(prev => prev + 1)}
      >
        Count
      </button>

      <Child onClick={handleClick} />
    </>
  );
}
```
Now suppose:
```
Count changes
```
Question:

Did `handleClick` logic change?
```
No.
```
But React created a new function object.

So Child receives:

Before:
```
onClick = Function A
```
After:
```
onClick = Function B
```
React sees:

Prop changed

So Child re-renders.

## useCallback Solution
```
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```
Now React does:

Render #1
```
Create Function A
```
Render #2
```
Reuse Function A
```
Render #3
```
Reuse Function A
```
The function reference stays the same.

### Syntax
```
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```
Notice something familiar?

Compare:

### useMemo
```
const total = useMemo(() => {
  return expensiveCalculation();
}, [count]);
```

### useCallback
```
const handleClick = useCallback(() => {
  console.log("Clicked");
}, []);
```
They have the same dependency array behavior.

The only difference is what they return.

## The Biggest Difference
### useMemo

Returns:
```
A VALUE
```
Example:
```
const total = useMemo(() => {
  return cart.reduce(...);
}, [cart]);
```
`total` is a number.

### useCallback

Returns:
```
A FUNCTION
```
Example:
```
const handleClick = useCallback(() => {
  console.log("Hello");
}, []);
```
`handleClick` is a function.

### Comparison Table

| Hook | Stores |
|------|--------|
| `useMemo` | Result of a calculation |
| `useCallback` | Function reference |

### When Should You Use It?

Here's something many tutorials don't tell beginners.

❌ Don't write:
```
const handleClick = useCallback(() => {
  console.log("Hello");
}, []);
```
**just because you can**.

If you're not passing that function to an optimized child (`React.memo`) or using it as a dependency somewhere, `useCallback` often adds unnecessary complexity.

Use it when it solves a real problem, not by default.

## Common Mistake

Beginners think:
```
useCallback makes functions faster.
```
❌ Not true.

It does not make the function execute faster.

It makes the function reference stable.

That's a huge difference.

## Visual Mental Model

### Without useCallback:
```
Render 1
 ↓
Function A

Render 2
 ↓
Function B

Render 3
 ↓
Function C
```
### With useCallback:
```
Render 1
 ↓
Function A

Render 2
 ↓
Function A

Render 3
 ↓
Function A
```
