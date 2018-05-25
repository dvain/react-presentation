# React 16
- what's new in React 16 
- ...and a tiny sneak peek on the new features we're going to get in React 17

---

## React 16 - outline
- fragments
- error boundaries
- portals
- custom DOM attributes
- server-side rendering

---
## well *hello* there @fa[user-astronaut]

![pool](https://media.giphy.com/media/26FLf3L9bDpYCVO5G/giphy.gif)

---
### fragments - what are fragments ?

- A common pattern is for a component to return a list of children. Take this example HTML:

```html
Some text.
<h2>A heading</h2>
More text.
<h2>Another heading</h2>
Even more text.
```
---

- Prior to version 16, the only way to achieve this in React was by wrapping the children in an extra element,
 usually a div or span.

```
render() {
  return (
    // Never forget the div! meh...
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
### fragments 2

- with react 16.0 we were now able to return an array of elements from a components render method. Instead of wrapping the children in a DOM element, you can put them into an array:

```
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
### fragments 3 - first-class fragment component
- but wait, it's all water under the bridge now. 
```
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
- You can use "Fragment" the same way you’d use any other element, without changing the way you write JSX. No commas, no keys, no quotes.
---

### error boundaries - why you will love them

- In the past, JavaScript errors inside components used to corrupt React’s internal state and cause it to emit cryptic errors on next renders. 
- These errors were always caused by an earlier error in the application code, but React did not provide a way to handle them gracefully in components, and could not recover from them.
- This is overcome in React 16 by the introduction of Error Boundaries

---

### error boundaries

- Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of the component tree that crashed.
- Error boundaries catch errors during rendering, in lifecycle methods, and in constructors of the whole tree below them.

---
### error boundaries - example

- A class component becomes an error boundary if it defines a new lifecycle method called componentDidCatch(error, info):

```
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
---
### error boundaries - example

- Then you can use it as a regular component:
```
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```
- The componentDidCatch() method works like a JavaScript catch {} block, but for components.
- Only class components can be error boundaries. 
- In practice, most of the time you’ll want to declare an error boundary component once and use it throughout your application.
- Note that error boundaries only catch errors in the components below them in the tree. 

---
### error-boundaries live demo
- [codepen example](https://codepen.io/gaearon/pen/wqvxGa?editors=0010)

---
### portals
- Portals provide a first-class way to render children into a DOM node that exists outside the DOM hierarchy of the parent component.
```
ReactDOM.createPortal(child, container)
```

- Normally, when you return an element from a component’s render method, it’s mounted into the DOM as a child of the nearest parent node

---
### portals
- However, sometimes it’s useful to insert a child into a different location in the DOM
```
render() {
  // React does *not* create a new div. It renders the children into `domNode`.
  // `domNode` is any valid DOM node, regardless of its location in the DOM.
  return ReactDOM.createPortal(
    this.props.children,
    domNode
  );
}
```
---
### portals 2 -use cases
- A typical use case for portals is when a parent component has an overflow: hidden or z-index style,
 but you need the child to visually “break out” of its container. For example, dialogs, hovercards, and tooltips.
- another exciting aspect of portals is event bubbling through them
- an event fired from inside a portal will propagate to ancestors in the containing react tree, even if those
aren't ancestors in the DOM tree
- [event bubbling example](https://codepen.io/gaearon/pen/jGBWpE)

---
### custom DOM attributes & server-side rendering
- Instead of ignoring unrecognized HTML and SVG attributes, React will now pass them through to the DOM. 
- React 16 includes a completely rewritten server renderer.
- According to Sasha Aickin's (core team member) synthetic benchmarks,
 server rendering in React 16 is roughly three times faster than React 15
---

#### React 16 "boiling hot" (react blog March-May 2018)

- "One of the biggest lessons we’ve learned is that some of our legacy component lifecycles tend to encourage unsafe coding practices. They are:
     - componentWillMount
     - componentWillReceiveProps
     - componentWillUpdate"
---
#### React 16 "boiling hot" (react blog March-May 2018)
- These lifecycle methods have often been misunderstood and subtly misused; furthermore, we anticipate that their potential misuse may be more problematic with async rendering. Because of this, we will be adding an “UNSAFE_” prefix to these lifecycles in an upcoming release. (Here, “unsafe” refers not to security but instead conveys that code using these lifecycles will be more likely to have bugs in future versions of React, especially once async rendering is enabled.)
---

#### The problems with componentWillReceiveProps in a nuthsell
* dev: 'Hang on, I use componentWillReceiveProps in just about every large non-function component I build.... I just counted 50 times in one of my applications.'
* Dan Abramov: 'Curious, what do you use it for?'
* dev: 'I use it to handle changes in props. For example load something from network, which goes into redux, redux triggers a props update, so then I do something because for example now redux state has changed to say "loaded === true" Is this not the correct function to handle changes in props?'
* [continue](https://news.ycombinator.com/item?id=16711813)
---
#### React 17

> 17.0: Remove componentWillMount, componentWillReceiveProps, and componentWillUpdate . (Only the new “UNSAFE_” lifecycle names will work from this point forward.)

---
#### React 16 
- Good reads:
- [New lifecycles and context API](https://reactjs.org/blog/2018/03/29/react-v-16-3.html)
- [Pointer events](https://reactjs.org/blog/2018/05/23/react-v-16-4.html)
---

#### React 17 teaser - Async Rendering
- a.r. is a generic way for components to suspend rendering while they load async data ("suspense")
- state update can be paused until data is ready
- on fast networks: fluid and instantaneous updates without tons a "spinner parade"
- on slow networks: we will be able to intentionally design whcih loading states the user should see (and their granularity)
- instead of showing spinners based on how the code is written: the app stays responsive throughout
- [Async Rendering](https://reactjs.org/blog/2018/03/27/update-on-async-rendering.html)
- [discussion on async rendering](https://news.ycombinator.com/item?id=16692981)

---
## Thank you!
![thanks](https://media.giphy.com/media/3otPoUkg3hBxQKRJ7y/giphy.gif)