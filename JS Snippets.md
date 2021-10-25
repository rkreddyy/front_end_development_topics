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

