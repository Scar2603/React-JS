# useMemo

Imagine your boss asks:
```
Calculate annual revenue
```
You spend:
```
10 minutes
```
calculating.

Then boss asks:
```
What's my name?
```
And you recalculate annual revenue again.

Waste of time.

React sometimes does the same thing.

Every re-render:
```
Component runs again
```
Even expensive calculations run again.

**Example**:
```
function App() {

  const [count, setCount] = useState(0);

  const total = expensiveCalculation();

  return (...);
}
```
Every click:
```
setCount(...)
```
causes:
```
Re-render
 ↓
expensiveCalculation()
 ↓
Runs again
```
Even if result never changed.

### useMemo Solution
```
const total = useMemo(() => {

  return expensiveCalculation();

}, []);
```
Meaning:
```
Calculate once
 ↓
Remember result
 ↓
Reuse it
```
The word Memo comes from:
```
Memoization
```
which means:
```
Remember previous result
```
instead of recalculating.

### Quick Question

Imagine:
```
const result = useMemo(() => {
  console.log("Calculating...");
  return 100;
}, []);
```
And component re-renders 20 times.

How many times will:
```
Calculating...
```
print?

**Answer**:
```
prints once.
```

Look at this:
```
const result = useMemo(() => {
  console.log("Calculating...");
  return 100;
}, []);
```
Dependency array:
```
[]
```
means:
```
Run once
Remember result
Reuse result forever
```
Therefore:
```
Initial Mount
 ↓
Calculating...
```
prints once.

Then suppose:
```
setCount(...)
```
causes:
```
20 re-renders
```
React does:
```
Component runs again
 ↓
Checks useMemo dependencies
 ↓
[]
 ↓
Nothing changed
 ↓
Reuse old value
```
No recalculation.

So:
```
Calculating...
```
prints:
```
1 time
```
only.

## Here's The Interesting Part

You already know:
```
useEffect(() => {}, [count])
```
runs again when:
```
count
```
changes.

` useMemo ` uses the exact same dependency logic.

### Example:
```
const result = useMemo(() => {

  console.log("Calculating...");

  return count * 10;

}, [count]);
```
Now:

### Initial Render
```
Calculating...
```
prints.

### Change name state
```
setName("John")
```
Component re-renders.

Dependency:
```
[count]
```
did not change.

Result:
```
No calculation
```
### Change count state
```
setCount(1)
```
Dependency changed.

Result:
```
Calculating...
```
prints again.

### Mental Model

Think:
```
useMemo
```
as:
```
Calculator + Memory
```
**Without useMemo**:
```
Re-render
 ↓
Calculate
 ↓
Re-render
 ↓
Calculate
 ↓
Re-render
 ↓
Calculate
```
**With useMemo**:
```
Re-render
 ↓
Check dependencies
 ↓
Changed?
   Yes → Calculate
   No  → Reuse old result
```

## Why Do We Need It?

Suppose:
```
function slowFunction() {

  console.log("Running...");

  for(let i = 0; i < 1000000000; i++) {}

  return 100;
}
```
Very expensive.

### Without useMemo:
```
const result = slowFunction();
```
Every render:
```
Wait...
Wait...
Wait...
```
**UI becomes slow**.

### With useMemo:
```
const result = useMemo(() => {
  return slowFunction();
}, []);
```
Runs once.

Fast afterwards.
