- Local variables and functions are not added to `global` scope in node.js
```
function sayHello(name){
  return `Hello ${name}`;
}

var greet = sayHello('Ravi');
console.log(greet); // Hello Ravi
console.log(global.greet); // undefined
console.log(global.sayHello); // undefined
```
- In node every file is a module and variables and functions defined in that file are scoped to that module
- While executing a file, node wraps all the content of the file inside a IIFE with some set of parameters and run the code
```
(function(exports, require, module, __filename, __dirname){
  // contents from the file placed here before executing the file
})
```
- 
