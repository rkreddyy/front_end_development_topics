# React JS Code Snippets

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


