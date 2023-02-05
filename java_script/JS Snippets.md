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

#### The for..in loop iterates over inherited properties too, but Object.keys does not

```
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// Object.keys only returns own keys
alert(Object.keys(rabbit)); // jumps

// for..in loops over both own and inherited keys
for(let prop in rabbit) alert(prop); // jumps, then eats
```
###### We can filter out inherited properties (or do something else with them):
```
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

for(let prop in rabbit) {
  let isOwn = rabbit.hasOwnProperty(prop);

  if (isOwn) {
    alert(`Our: ${prop}`); // Our: jumps
  } else {
    alert(`Inherited: ${prop}`); // Inherited: eats
  }
}
```
