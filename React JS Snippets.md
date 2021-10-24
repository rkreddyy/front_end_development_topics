# React JS Code Snippets

### What will the following component output?
#### Function gets called being every time the component renders.
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

### 
#### 
