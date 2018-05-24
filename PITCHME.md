# React 16 Exciting new stuff

Presentation on what's new in React 16 

...and what tremendous new features we're going to witness in React 17

---

### you ready?

![pool](https://media.giphy.com/media/26FLf3L9bDpYCVO5G/giphy.gif)

---

## React 16
- fragments
- error boundaries
- portals
- custom DOM attributes
- server-side rendering

---
## fragments 1 - what are fragments ?

- A common pattern is for a component to return a list of children. Take this example HTML:

```html
Some text.
<h2>A heading</h2>
More text.
<h2>Another heading</h2>
Even more text.
```

-Prior to version 16, the only way to achieve this in React was by wrapping the children in an extra element, usually a div or span:

```jsx harmony
render() {
  return (
    // Extraneous div element :(
    <div>
      Some text.
      <h2>A heading</h2>
      More text.
      <h2>Another heading</h2>
      Even more text.
    </div>
  );
}
```
---
## fragments 2

To address this limitation, React 16.0 added support for returning an array of elements from a component’s render method. Instead of wrapping the children in a DOM element, you can put them into an array:

- with react 16.0 we were now able to return an array of elemnts from a components render method.

```jsx harmony

render() {
  // No need to wrap list items in an extra element!
  return [
    // the infamous keys!
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
}

```
---
## fragments 3

- but please ignore that!
- what is possible since react 16.2:

>"To provide a more consistent authoring experience for fragments, React now provides a first-class Fragment component that can be used in place of arrays."

```jsx harmony
render() {
  return (
    <Fragment>
      Some text.
      <h2>A heading</h2>
      More text.
      <h2>Another heading</h2>
      Even more text.
    </Fragment>
  );
}

```
- You can use \<Fragment /> the same way you’d use any other element, without changing the way you write JSX. No commas, no keys, no quotes.

---
## error boundaries - why you will love them

- In the past, JavaScript errors inside components used to corrupt React’s internal state and cause it to emit cryptic errors on next renders. 
- These errors were always caused by an earlier error in the application code, but React did not provide a way to handle them gracefully in components, and could not recover from them.
- This is overcome in React 16 by the introduction of Error Boundaries

- Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed.
- Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.

## error boundaries - 2 example

- A class component becomes an error boundary if it defines a new lifecycle method called componentDidCatch(error, info):

```jsx harmony
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  componentDidCatch(error, info) {
    // Display fallback UI
    this.setState({ hasError: true });
    // You can also log the error to an error reporting service
    logErrorToMyService(error, info);
  }

  render() {
    if (this.state.hasError) {
      // You can render any custom fallback UI
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

- Then you can use it as a regular component:


```jsx harmony
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

- The componentDidCatch() method works like a JavaScript catch {} block, but for components. Only class components can be error boundaries. In practice, most of the time you’ll want to declare an error boundary component once and use it throughout your application.

- Note that error boundaries only catch errors in the components below them in the tree. 

- [codepen example](https://codepen.io/gaearon/pen/wqvxGa?editors=0010)

---
## portals

---
## custom DOM attributes

---
## server-side rendering
---

## React 16 "boiling hot"

- [New lifecycles and context API](https://reactjs.org/blog/2018/03/29/react-v-16-3.html)
- [Pointer events](https://reactjs.org/blog/2018/05/23/react-v-16-4.html)
---


## React 17 teaser
- 
- [Sneak Peek: Beyond React 16](https://reactjs.org/blog/2018/03/01/sneak-peek-beyond-react-16.html)
- [Async Rendering](https://reactjs.org/blog/2018/03/27/update-on-async-rendering.html)

---