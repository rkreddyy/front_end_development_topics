# React JS Core Concepts With Example Code

### Code splitting with React.lazy and Suspense
```
import React, { lazy, Suspense } from 'react';

const AvatarComponent = lazy(() => import('./AvatarComponent'));
const InfoComponent = lazy(() => import('./InfoComponent'));
const MoreInfoComponent = lazy(() => import('./MoreInfoComponent'));

const renderLoader = () => <p>Loading</p>;

const DetailsComponent = () => (
  <Suspense fallback={renderLoader()}>
    <AvatarComponent />
    <InfoComponent />
    <MoreInfoComponent />
  </Suspense>
)
```

### Route based code splitting
```
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  </Router>
);
```

### Custom hook that detects whether you are online or offline
```
import React from 'react';

function useOffline() {
  const [isOffline, setIsOffline] = React.useState(false);

  function onOffline() {
    setIsOffline(true);
  }

  function onOnline() {
    setIsOffline(false);
  }

  React.useEffect(() => {
    window.addEventListener('offline', onOffline);
    window.addEventListener('online', onOnline);

    return () => {
      window.removeEventListener('offline', onOffline);
      window.removeEventListener('online', onOnline);
    };
  }, []);

  return isOffline;
}

function App() {
  const isOffline = useOffline();

  if (isOffline) {
    return <div>Sorry, you are offline ...</div>;
  }

  return <div>You are online!</div>;
}

export default App;
```

### Controlled/Un-controlled component
This relates to stateful DOM components (form elements) and the React docs explain the difference:

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

### ASYNCHRONOUS STATE IN REACT
##### What happens if you are dependent on actual state to update the state?
```
const App = () => {
  const [count, setCount] = React.useState(0);

  const handleIncrease = () => {
    setTimeout(() => setCount(count + 1), 1000);
  };

  const handleDecrease = () => {
    setTimeout(() => setCount(count - 1), 1000);
  };

  return (
    <div>
      Count: {count}
      <hr />
      <div>
        <button type="button" onClick={handleIncrease}>
          Increase
        </button>
        <button type="button" onClick={handleDecrease}>
          Decrease
        </button>
      </div>
    </div>
  );
};
```
Every time you click on one of the buttons, the state update function is called with a delay of one second. That works for a single click. However, try to click one of the buttons multiple times in a row. The state update function will always operate on the same state (here: count) within this one second. In order to fix this problem, you can pass a function to the state update function from useState:
```
import React from 'react';

const App = () => {
  const [count, setCount] = React.useState(0);

  const handleIncrease = () => {
    setTimeout(() => setCount(state => state + 1), 1000);
  };

  const handleDecrease = () => {
    setTimeout(() => setCount(state => state - 1), 1000);
  };

  return (
    <div>
      Count: {count}
      <hr />
      <div>
        <button type="button" onClick={handleIncrease}>
          Increase
        </button>
        <button type="button" onClick={handleDecrease}>
          Decrease
        </button>
      </div>
    </div>
  );
};

export default App;
```
The function offers you the state at the time of executing the function. This way, you never operate on any stale state. Therefore a good rule of thumb may be: Always use a function in useState's update function if your state update depends on your previous state.

### Key prop can't be passed to child component with prop name `key`
```
const List = ({ users }) => (
  <ul>
    {users.map(user => <Item key={user.id}>{user.name}</Item>)}
  </ul>
);

const Item = ({ key, children }) => (
  <p>{key} {children}</p>
);
```
You will also see a warning in your developer console log: "... key is not a prop. Trying to access it will result in undefined being returned. In this case, you have to pass a second prop when you want to get the key from the props.
The workaround to pass props (e.g. key) which are internally used by React and not passed to the child components.
```
const List = ({ users }) => (
  <ul>
    {users.map(user => (
      <Item key={user.id} id={user.id}>
        {user.name}
      </Item>
    ))}
  </ul>
);

const Item = ({ id, children }) => (
  <p>{id} {children}</p>
);
```

### Pass props to Styled Components
They can be used for styling your components in React. Rather than thinking about cascading style sheets as for HTML styles, you only style your components. So the style becomes more co-located to your components. In fact, in the case of styled components, the style becomes a React component:
```
import styled from 'styled-components';

const Input = styled.input`
  padding: 0.5em;
  margin: 0.5em;
  color: palevioletred;
  background: papayawhip;
  border: none;
  border-radius: ${props => props.hasRadius ? '3px' : '0px'};
`;

const App = () => {
  const [value, setValue] = React.useState('');

  const onChange = (event) => {
    setValue(event.target.value);
  }

  return (
    <div>
      <Input
        value={value}
        onChange={onChange}
      />
    </div>
  );
}
```

### What will the following component output?
#### Function gets called every time the component renders.
`onClick={incrementCount()}` should be changed to `onClick={incrementCount}` in order for the component to work properly.
Make sure you aren’t calling the function when you pass it to the component. Instead, pass the function itself (without parens)
```
import {useState} from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  let incrementCount = (event) => {
    setCount(count + 1);
  };

  return (
    <div>
      <button onClick={incrementCount()}>Count: {count}</button>
    </div>
  );
}
```

### The props spreading
The props spreading can be used to spread a whole object with key value pairs down to a child component. It has the same effect as passing each property of the object property by property to the component. For example, sometimes you have a component in between which does not care about the props and just passes them along to the next component:

```
import * as React from 'react';

const App = () => {
  const title = 'React';
  const description = 'Your component library for ...';

  return (
    <div>
      <Welcome title={title} description={description} />
    </div>
  );
};

const Welcome = (props) => {
  return  <Message {...props} />;
};

const Message = ({ title, description }) => {
  return (
    <div>
      <h1>{title}</h1>
      <p>{description}</p>
    </div>
  );
}

export default App;
```
Be aware that the spreaded attribute/value pairs can be overridden as well:
```
const Welcome = (props) => {
  return (
    <div>
      <Message {...props} title="JavaScript" />
    </div>
  );
};

// Message prints title "JavaScript"
```

If the props spreading comes last, all the previous attributes get overridden if they are present in the props:
```
const Welcome = (props) => {
  return (
    <div>
      <Message title="JavaScript" {...props} />
    </div>
  );
};

// Message prints title "React"
```

### Children prop
The children prop in React can be used for composing React components into each other. Because of this feature, you can put JavaScript primitives or JSX between the opening and closing element's tags:
```
import * as React from 'react';

const App = () => {
  const [count, setCount] = React.useState(0);

  return (
    <div>
      <Button onClick={() => setCount(count + 1)}>
        {count}
      </Button>
    </div>
  );
};

const Button = ({ onClick, children }) => (
  <button onClick={onClick}>{children}</button>
);

export default App;
```

### HOW TO PASS COMPONENTS AS PROPS
```
const User = ({ user }) => (
  <Profile user={user}>
    <AvatarRound user={user} />
  </Profile>
);

const Profile = ({ user, children }) => (
  <div className="profile">
    <div>{children}</div>
    <div>
      <p>{user.name}</p>
    </div>
  </div>
);

const AvatarRound = ({ user }) => (
  <img className="round" alt="avatar" src={user.avatarUrl} />
);
```

However, what if you want to pass more than one React element and place them at different positions? Then again you don't need to use the children prop, because you have only one of them, and instead you just use regular props:
```
const User = ({ user }) => (
  <Profile
    user={user}
    avatar={<AvatarRound user={user} />}
    biography={<BiographyFat user={user} />}
  />
);

const Profile = ({ user, avatar, biography }) => (
  <div className="profile">
    <div>{avatar}</div>
    <div>
      <p>{user.name}</p>
      {biography}
    </div>
  </div>
);

const AvatarRound = ({ user }) => (
  <img className="round" alt="avatar" src={user.avatarUrl} />
);

const BiographyFat = ({ user }) => (
  <p className="fat">{user.biography}</p>
);
```

### CHILDREN AS A FUNCTION
#### [React Render Props](https://www.robinwieruch.de/react-render-props/)
The concept of children as a function or child as a function, also called render prop, is one of the advanced patterns in React (next to higher-order components). The components which implement this pattern can be called render prop components.

First, let's start with the render prop. Basically it is a function passed as prop. The function receives parameters (in this case the amount), but also renders JSX (in this case the components for the currency conversion).

```
import * as React from 'react';

const App = () => (
  <div>
    <h1>US Dollar to Euro:</h1>
    <Amount toCurrency={(amount) => <Euro amount={amount} />} />

    <h1>US Dollar to Pound:</h1>
    <Amount toCurrency={(amount) => <Pound amount={amount} />} />
  </div>
);

const Amount = ({ toCurrency }) => {
  const [amount, setAmount] = React.useState(0);

  const handleIncrement = () => setAmount(amount + 1);
  const handleDecrement = () => setAmount(amount - 1);

  return (
    <div>
      <button type="button" onClick={handleIncrement}>
        +
      </button>
      <button type="button" onClick={handleDecrement}>
        -
      </button>

      <p>US Dollar: {amount}</p>
      {toCurrency(amount)}
    </div>
  );
};

const Euro = ({ amount }) => <p>Euro: {amount * 0.86}</p>;

const Pound = ({ amount }) => <p>Pound: {amount * 0.76}</p>;

export default App;
```


### useReducer example
```
import React, {useState, useReducer} from 'react'
import axios from 'axios'

const initialState = {
  error: null,
  greeting: null,
}

function greetingReducer(state, action) {
  switch (action.type) {
    case 'SUCCESS': {
      return {
        error: null,
        greeting: action.greeting,
      }
    }
    case 'ERROR': {
      return {
        error: action.error,
        greeting: null,
      }
    }
    default: {
      return state
    }
  }
}

export default function Fetch({url}) {
  const [greetingState, dispatch] = useReducer(
    greetingReducer,
    initialState,
  )
  const [buttonClicked, setButtonClicked] = useState(false);
  const {error, greeting} = greetingState;

  const fetchGreeting = async url =>
    axios
      .get(url)
      .then(response => {
        const {data} = response
        const {greeting} = data
        dispatch({type: 'SUCCESS', greeting})
        setButtonClicked(true)
      })
      .catch(error => {
        dispatch({type: 'ERROR', error})
      })

  const buttonText = buttonClicked ? 'Ok' : 'Load Greeting'

  return (
    <div>
      <button onClick={() => fetchGreeting(url)} disabled={buttonClicked}>
        {buttonText}
      </button>
      {greeting && <h1>{greeting}</h1>}
      {error && <p role="alert">Oops, failed to fetch!</p>}
    </div>
  )
}
```

### Error boundaries
In the past, JavaScript errors inside components used to corrupt React’s internal state and cause it to emit cryptic errors on next renders. These errors were always caused by an earlier error in the application code, but React did not provide a way to handle them gracefully in components, and could not recover from them.
```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // Update state so the next render will show the fallback UI.
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // You can also log the error to an error reporting service
    logErrorToMyService(error, errorInfo);
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
Usage 
```
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

### Typechecking With PropTypes
```
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};

/// Function component
function HelloWorldComponent({ name }) {
  return (
    <div>Hello, {name}</div>
  )
}

HelloWorldComponent.propTypes = {
  name: PropTypes.string
}

export default HelloWorldComponent

/// Default props
class Greeting extends React.Component {
  static defaultProps = {
    name: 'stranger'
  }

  render() {
    return (
      <div>Hello, {this.props.name}</div>
    )
  }
}

// Function component
function Greeting({ name }) {
  return <div>Hello, {name}</div>;
}
Greeting.defaultProps = {
  name: "stranger"
};
```


