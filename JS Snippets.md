# JS Problem Solving Snippets

#### In the browser context, if the code is executing in strict mode value of this is undefined. Else it is window object in the function execution context.
```
function hello(){
    'use strict'
    console.log(this);
}
hello();
// prints undefined

////////////////////////////////////

function hello(){
    console.log(this);
}
hello();
// prints window object
```

#### Insert an element into an array at a given index.

```
const numbers = [1,2,4];
const index = 2;

const finalNumbers = [...numbers.slice(0, index), 3, ...numbers.slice(index)];
console.log(finalNumbers);
```
