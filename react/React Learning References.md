- Redux time travel debugging.
- [Optimize your React app](https://web.dev/get-started-optimize-react/)
- [JavaScript fundamentals before learning React](https://www.robinwieruch.de/javascript-fundamentals-react-requirements/)
- [How to CSS Style in React](https://www.robinwieruch.de/react-css-styling/)
- [React Styled Components Tutorial](https://www.robinwieruch.de/react-styled-components/)
- [React Higher-Order Components (HOCs)](https://www.robinwieruch.de/react-higher-order-components/)
  - Higher-Order Components take any React component as input component and return an enhanced version of it as output component (Higher Order Component => EnhancedComponent).
- [React Render Props/Composition](https://www.robinwieruch.de/react-render-props/)
  - The concept of children as a function or child as a function, also called render prop in general, is one of the advanced patterns in React (next to higher-order components). The components which implement this pattern could be called render prop components.
- [How to use Props in React](https://www.robinwieruch.de/react-pass-props-to-component/)
- [How to use React State](https://www.robinwieruch.de/react-state/)
- [What are Controlled Components in React](https://www.robinwieruch.de/react-controlled-components/)
  - This relates to stateful DOM components (form elements) and the React docs explain the difference:
A Controlled Component is one that takes its current value through props and notifies changes through callbacks like onChange. A parent component "controls" it by handling the callback and managing its own state and passing the new values as props to the controlled component. You could also call this a "dumb component".
A Uncontrolled Component is one that stores its own state internally, and you query the DOM using a ref to find its current value when you need it. This is a bit more like traditional HTML.
Most native React form components support both controlled and uncontrolled usage:

```
// Controlled:
<input type="text" value={value} onChange={handleChange} />

// Uncontrolled:
<input type="text" defaultValue="foo" ref={inputRef} />
// Use `inputRef.current.value` to read the current value of <input>
```
- [How to useState in React](https://www.robinwieruch.de/react-usestate-hook/)
- [How to useEffect in React](https://www.robinwieruch.de/react-useeffect-hook/)
- [How to use React Ref](https://www.robinwieruch.de/react-ref/)
  - The useRef Hook returns us a mutable object which stays intact over the lifetime of a React component. Specifically, the returned object has a current property which can hold any modifiable value for us:
- [Computed Properties in React](https://www.robinwieruch.de/react-computed-properties/)
- [React Conditional Rendering](https://www.robinwieruch.de/conditional-rendering-react/)
- [React Styled Components Tutorial](https://www.robinwieruch.de/react-styled-components/)
- [How to use React Context](https://www.robinwieruch.de/react-context/)
- [React Hooks Tutorial](https://www.robinwieruch.de/react-hooks/)
- [How to fetch data with React Hooks](https://www.robinwieruch.de/react-hooks-fetch-data/)
- [Why React Hooks over HOCs](https://www.robinwieruch.de/react-hooks-higher-order-components/)
- [React State Hooks: useReducer, useState, useContext](https://www.robinwieruch.de/react-state-usereducer-usestate-usecontext/)
- [How to create Redux with React Hooks?](https://www.robinwieruch.de/redux-with-react-hooks/)
- [React Redux Tutorial for Beginners](https://www.robinwieruch.de/react-redux-tutorial/)
- [A Firebase in React Tutorial for Beginners [2019]](https://www.robinwieruch.de/complete-firebase-authentication-react-tutorial/)
- [React Libraries for 2022](https://www.robinwieruch.de/react-libraries/)
- [A complete React with GraphQL Tutorial](https://www.robinwieruch.de/react-with-graphql-tutorial/)
