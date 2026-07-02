# React.memo

Real-world Analogy

Imagine your family is watching TV.

Your younger brother is drawing on paper.

Now your phone rings.

You answer the phone.

Question:

Should your brother redraw the entire picture?
```
You answered phone
      ↓
Brother redraws picture
```
🤦 Makes no sense.

He should continue drawing only if his work changed.

React has the same problem.

### The Problem

Consider:
```
function Parent() {

    const [count, setCount] = useState(0);

    return (
        <>
            <button
                onClick={() => setCount(prev => prev + 1)}
            >
                Increase
            </button>

            <Child />
        </>
    );
}
```
Child:
```
function Child() {

    console.log("Child Rendered");

    return <h1>Child</h1>;
}
```
### Predict

Suppose you click:
```
Increase
```
5 times.

Question:

How many times does:
```
Child Rendered
```
print?

### Most Beginners Think
```
Child didn't change
      ↓
Child should not render
```
❌ Wrong.

### What Actually Happens

Remember what you've learned:
```
State changes
      ↓
Parent re-renders
```
When Parent re-renders:
```
<Child />
```
is evaluated again.

So Child also renders.

Console:
```
Child Rendered
Child Rendered
Child Rendered
Child Rendered
Child Rendered
Child Rendered
```
(1 initial + 5 clicks)

### Why?

React's default behavior is:
```
Parent renders
      ↓
Children render
```
## React.memo Solution

We tell React:
```
"If Child's props didn't change,
don't render it."
```
How?
```
const Child = React.memo(function Child() {

    console.log("Child Rendered");

    return <h1>Child</h1>;

});
```
That's it.

## What React.memo Does

### Without it:
```
Parent Render
      ↓
Child Render
```
Always.

With it:
```
Parent Render
      ↓
Did Child props change?

YES → Render Child

NO → Skip Child
```
### Example

**Parent**:
```
function Parent() {

    const [count, setCount] = useState(0);

    return (
        <>
            <button
                onClick={() =>
                    setCount(prev => prev + 1)
                }
            >
                Increase
            </button>

            <Child />
        </>
    );
}
```
**Child**:
```
const Child = React.memo(() => {

    console.log("Child Rendered");

    return <h2>Child</h2>;

});
```
Now click Increase 10 times.

Console:
```
Child Rendered
```
Only once.

### How Does React.memo Know?

React compares props.

Suppose:

Old props:
```
{
  name: "Alice"
}
```
New props:
```
{
  name: "Alice"
}
```

React says:
```
Same props
↓
Skip render
```
Now:

Old:
```
{
  name: "Alice"
}
```
New:
```
{
  name: "Bob"
}
```
React says:
```
Props changed
↓
Render Child
```
### Now The Missing Puzzle Piece

Remember:
```
function Parent() {

    function handleClick() {}

    return (
        <Child onClick={handleClick} />
    );
}
```
Without `useCallback`:

Render #1
```
Function A
```
Render #2
```
Function B
```
React.memo compares:
```
Old onClick = Function A

New onClick = Function B
```
Different.

So:
```
Child re-renders
```
Even though the logic didn't change.

### With useCallback
```
const handleClick = useCallback(() => {

}, []);
```
Now:

Render #1
```
Function A
```
Render #2
```
Function A
```
React.memo compares:
```
Old = Function A

New = Function A
```
Same.

Skip render.

### The Complete Picture

Without optimization:
```
State Change
      ↓
Parent Render
      ↓
New Function Created
      ↓
Child Props Changed
      ↓
Child Render
```
With both:
```
State Change
      ↓
Parent Render
      ↓
useCallback
      ↓
Same Function
      ↓
React.memo
      ↓
Props Same
      ↓
Child Skip
```
Now `useCallback` finally has a purpose.
