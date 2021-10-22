# React JS Code Snippets

### What will the following component output?
#### It will be re-rendered repeatedly. `onClick={incrementCount()}` should be changed to `onClick={incrementCount}` in order for the component to work properly.
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
