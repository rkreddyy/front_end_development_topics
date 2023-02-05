### The “property descriptor” object contains the value and all the flags.
```
let user = {
  name: "John"
};

let descriptor = Object.getOwnPropertyDescriptor(user, 'name');

alert( JSON.stringify(descriptor, null, 2 ) );
/* property descriptor:
{
  "value": "John",
  "writable": true,
  "enumerable": true,
  "configurable": true
}
*/
```


```
let user = {
  name: "John"
};

Object.defineProperty(user, "name", {
  writable: false,
  enumerable: true,
  configurable: false
});

user.name = "Pete"; // Error: Cannot assign to read only property 'name'
delete user.name won't work either - non configurable
```

```
// Define all properties at once
Object.defineProperties(user, {
  name: { value: "John", writable: false },
  surname: { value: "Smith", writable: false },
  // ...
});
```

### Sealing an object globally
Property descriptors work at the level of individual properties.

There are also methods that limit access to the whole object:

#### Object.preventExtensions(obj)
Forbids the addition of new properties to the object.
#### Object.seal(obj)
Forbids adding/removing of properties. Sets configurable: false for all existing properties.
#### Object.freeze(obj)
Forbids adding/removing/changing of properties. Sets configurable: false, writable: false for all existing properties.
And also there are tests for them:

#### Object.isExtensible(obj)
Returns false if adding properties is forbidden, otherwise true.
#### Object.isSealed(obj)
Returns true if adding/removing properties is forbidden, and all existing properties have configurable: false.
#### Object.isFrozen(obj)
Returns true if adding/removing/changing properties is forbidden, and all current properties are configurable: false, writable: false.
