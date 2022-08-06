# Node JS

#### Simple Node server
```
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

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
###### While executing a file, node wraps all the content of the file inside a IIFE with some set of parameters and run the code
```
(function(exports, require, module, __filename, __dirname){
  // contents from the file placed here before executing the file
})
```
#### 

## Q&A
##### For which kind of apllications Node is best suited for?
It is ideal to use Node.js for developing streaming or event-based real-time applications that require less CPU usage such as chat apps, game servers, collaborate env apps, Advertisement servers, streaming servers. 

##### What is the difference between process.nextTick() and setImmediate()? 
A function passed to process.nextTick() is going to be executed on the current iteration of the event loop, after the current operation ends. This means it will always execute before setTimeout and setImmediate.

A setTimeout() callback with a 0ms delay is very similar to setImmediate(). The execution order will depend on various factors, but they will be both run in the next iteration of the event loop.

##### [How to solve “Process out of Memory Exception” in Node.js](https://www.geeksforgeeks.org/how-to-solve-process-out-of-memory-exception-in-node-js/#:~:text=Process%20out%20of%20Memory%20Exception%20is%20an%20exception%20that%20occurs,by%20our%20program%20while%20execution.)
Process out of Memory Exception is an exception that occurs when your node.js program gets out of memory. This happens when the default memory allocated to our program gets exceeded by our program while execution. 

`node --max-old-space-size=<SPACE_REQUIRED> index.js`

##### How many types of streams are present in node.js?
Streams are objects that let you read data from a source or write data to a destination in continuous fashion. There are four types of streams 

1. Readable − Stream which is used for read operation. 
2. Writable − Stream which is used for write operation. 
3. Duplex − Stream which can be used for both read and write operation. 
4. Transform − A type of duplex stream where the output is computed based on input. 

Each type of Stream is an instance of EventEmitter and throws several events at different instance of times. 

- data − This event is fired when there is data is available to read. 
- end − This event is fired when there is no more data to read. 
- error − This event is fired when there is any error receiving or writing data. 
- finish − This event is fired when all the data has been flushed to underlying system. 

```
var fs = require("fs"); 
var data = ''; 

// Create a readable stream 
var readerStream = fs.createReadStream('input.txt'); 

// Set the encoding to be utf8.  
readerStream.setEncoding('UTF8'); 

// Handle stream events --> data, end, and error 
readerStream.on('data', function(chunk) {
   data += chunk; 
});

readerStream.on('end',function() {
   console.log(data); 
});

readerStream.on('error', function(err) {
   console.log(err.stack);
});

console.log("Program Ended"); 
```


