# Node JS

#### [The Node.js Event Loop, Timers, and process.nextTick()](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)
#### [REPL](https://nodejs.dev/learn/how-to-use-the-nodejs-repl)
REPL stands for Read Evaluate Print Loop, and it is a programming language environment (basically a console window) that takes single expression as user input and returns the result back to the console after execution. The REPL session provides a convenient way to quickly test simple JavaScript code.

Type node and hit `enter` key on command line to activate REPL
```
❯ node
>
```
The REPL is waiting for us to enter some JavaScript code, to be more precise.
Start simple and enter
```
> console.log('test')
test
undefined
>
```


We can import the REPL in a JavaScript file using repl.
```const repl = require('repl');```
Using the repl variable we can perform various operations. To start the REPL command prompt, type in the following line.
```repl.start();```

#### Understanding process.nextTick()
As you try to understand the Node.js event loop, one important part of it is process.nextTick().

Every time the event loop takes a full trip, we call it a tick.

When we pass a function to process.nextTick(), we instruct the engine to invoke this function at the end of the current operation, before the next event loop tick starts:
```
process.nextTick(() => {
  // do something
});
```
The event loop is busy processing the current function code.

When this operation ends, the JS engine runs all the functions passed to nextTick calls during that operation.

It's the way we can tell the JS engine to process a function asynchronously (after the current function), but as soon as possible, not queue it.

Calling setTimeout(() => {}, 0) will execute the function at the end of next tick, much later than when using nextTick() which prioritizes the call and executes it just before the beginning of the next tick.

Use nextTick() when you want to make sure that in the next event loop iteration that code is already executed.

#### [The Node.js fs module](https://nodejs.dev/learn/the-nodejs-fs-module)
#### [The Node.js os module](https://nodejs.dev/learn/the-nodejs-os-module)
#### [The Node.js events module](https://nodejs.dev/learn/the-nodejs-events-module)
#### [The Node.js http module](https://nodejs.dev/learn/the-nodejs-http-module)
#### [Node.js Buffers](https://nodejs.dev/learn/nodejs-buffers)
#### [Node.js Streams](https://nodejs.dev/learn/nodejs-streams)
#### [How to read environment variables from Node.js](https://nodejs.dev/learn/how-to-read-environment-variables-from-nodejs)
#### [Guides - Official Docs](https://nodejs.org/en/docs/guides/)
#### Local variables and functions are not added to `global` scope in node.js
```
function sayHello(name){
  return `Hello ${name}`;
}

var greet = sayHello('Ravi');
console.log(greet); // Hello Ravi
console.log(global.greet); // undefined
console.log(global.sayHello); // undefined
```
#### In node every file is a module and variables and functions defined in that file are scoped to that module
#### While executing a file, node wraps all the content of the file inside a IIFE with some set of parameters and run the code
```
(function(exports, require, module, __filename, __dirname){
  // contents from the file placed here before executing the file
})
```
#### 
