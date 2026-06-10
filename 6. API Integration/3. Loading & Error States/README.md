# Loading & Error States

Right now your API component has a problem.

Suppose internet is slow.

For 3 seconds:
```
Screen is empty
```
User sees:
```
Nothing
```
Bad UX.

Suppose API fails.
```
404
Network Error
Server Down
```
User still sees:
```
Nothing
```
Also bad.

Professional React applications always handle:
```
Loading
Success
Error
```
States.

## Real World Analogy

Ordering food:
```
Loading:
"Finding restaurants..."
```
```
Success:
Restaurant list appears
```
```
Error:
"Could not load restaurants"
```
## React Version

We'll use three pieces of state.
```
const [posts, setPosts] = useState([]);

const [loading, setLoading] =
    useState(true);

const [error, setError] =
    useState(null);
```
Meaning:
```
posts
Actual data
```
```
loading
Is request still running?
```
```
error
Did something fail?
```
## Professional Fetch Pattern
```
useEffect(() => {

    async function getPosts() {

        try {

            const response =
                await fetch(URL);

            const data =
                await response.json();

            setPosts(data);

        } catch (error) {

            setError(error.message);

        } finally {

            setLoading(false);

        }
    }

    getPosts();

}, []);
```

## New Keywords
**try**

Attempt operation.
```
try {
   ...
}
```
**catch**

Handle failure.
```
catch(error) {
   ...
}
```
**finally**

Always runs.

Success or failure.
```
finally {
   ...
}
```
Perfect for:
```
setLoading(false)
```
### Conditional Rendering

Now everything we've learned comes together.
```
if (loading) {
    return <h1>Loading...</h1>;
}

if (error) {
    return <h1>Error: {error}</h1>;
}
```
Only if both are false:
```
return (...)
```
show posts.
