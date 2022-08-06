# Express - Fast, unopinionated, minimalist web framework for Node.js

#### Simple express server setup
This app starts a server and listens on port 3000 for connections. The app responds with “Hello World!” for requests to the root URL (/) or route. For every other path, it will respond with a 404 Not Found.
```
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

#### [Routing](https://expressjs.com/en/guide/routing.html)
```
const express = require('express')
const app = express()

// GET method route
app.get('/', (req, res) => {
  res.send('GET request to the homepage')
})

// POST method route
app.post('/', (req, res) => {
  res.send('POST request to the homepage')
})
```
There is a special routing method, app.all(), used to load middleware functions at a path for all HTTP request methods. For example, the following handler is executed for requests to the route “/secret” whether using GET, POST, PUT, DELETE, or any other HTTP request method supported in the http module.
```
app.all('/secret', (req, res, next) => {
  console.log('Accessing the secret section ...')
  next() // pass control to the next handler
})
```
**Route parameters**
```
// Request URL: http://localhost:3000/users/34/books/8989
// req.params: { "userId": "34", "bookId": "8989" }
app.get('/users/:userId/books/:bookId', (req, res) => {
  res.send(req.params)
})
```
**Route handlers**
You can provide multiple callback functions that behave like middleware to handle a request. 
```
const cb0 = function (req, res, next) {
  console.log('CB0')
  next()
}

const cb1 = function (req, res, next) {
  console.log('CB1')
  next()
}

const cb2 = function (req, res) {
  res.send('Hello from C!')
}

app.get('/example/c', [cb0, cb1, cb2])
```
**app.route()**
You can create chainable route handlers for a route path by using app.route(). Because the path is specified at a single location, creating modular routes is helpful, as is reducing redundancy and typos. 
```
app.route('/book')
  .get((req, res) => {
    res.send('Get a random book')
  })
  .post((req, res) => {
    res.send('Add a book')
  })
  .put((req, res) => {
    res.send('Update the book')
  })
```
**express.Router**
Use the express.Router class to create modular, mountable route handlers. A Router instance is a complete middleware and routing system; for this reason, it is often referred to as a “mini-app”.

The following example creates a router as a module, loads a middleware function in it, defines some routes, and mounts the router module on a path in the main app.

Create a router file named birds.js in the app directory, with the following content:
```
const express = require('express')
const router = express.Router()

// middleware that is specific to this router
router.use((req, res, next) => {
  console.log('Time: ', Date.now())
  next()
})
// define the home page route
router.get('/', (req, res) => {
  res.send('Birds home page')
})
// define the about route
router.get('/about', (req, res) => {
  res.send('About birds')
})

module.exports = router
```
Then, load the router module in the app:
```
const birds = require('./birds')

// ...

app.use('/birds', birds)
```
The app will now be able to handle requests to /birds and /birds/about, as well as call the timeLog middleware function that is specific to the route.

#### [Writing middleware for use in Express apps](https://expressjs.com/en/guide/writing-middleware.html)
Middleware functions are functions that have access to the request object (req), the response object (res), and the next function in the application’s request-response cycle. The next function is a function in the Express router which, when invoked, executes the middleware succeeding the current middleware.

Middleware functions can perform the following tasks:
- Execute any code.
- Make changes to the request and the response objects.
- End the request-response cycle.
- Call the next middleware in the stack.
If the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging.

#### [Using middleware](https://expressjs.com/en/guide/using-middleware.html)

An Express application can use the following types of middleware:

- Application-level middleware
- Router-level middleware
- Error-handling middleware
- Built-in middleware
- Third-party middleware


**Application-level middleware**
Bind application-level middleware to an instance of the app object by using the app.use() and app.METHOD() functions, where METHOD is the HTTP method of the request that the middleware function handles (such as GET, PUT, or POST) in lowercase.
This example shows a middleware function with no mount path. The function is executed every time the app receives a request.
```
const express = require('express')
const app = express()

app.use((req, res, next) => {
  console.log('Time:', Date.now())
  next()
})
```
This example shows a middleware function mounted on the /user/:id path. The function is executed for any type of HTTP request on the /user/:id path.
```
app.use('/user/:id', (req, res, next) => {
  console.log('Request Type:', req.method)
  next()
})
```
This example shows a route and its handler function (middleware system). The function handles GET requests to the /user/:id path.
```
app.get('/user/:id', (req, res, next) => {
  res.send('USER')
})
```

**Router-level middleware**
Router-level middleware works in the same way as application-level middleware, except it is bound to an instance of express.Router().
```
const express = require('express')
const app = express()
const router = express.Router()

// a middleware function with no mount path. This code is executed for every request to the router
router.use((req, res, next) => {
  console.log('Time:', Date.now())
  next()
})

// a middleware sub-stack shows request info for any type of HTTP request to the /user/:id path
router.use('/user/:id', (req, res, next) => {
  console.log('Request URL:', req.originalUrl)
  next()
}, (req, res, next) => {
  console.log('Request Type:', req.method)
  next()
})

// a middleware sub-stack that handles GET requests to the /user/:id path
router.get('/user/:id', (req, res, next) => {
  // if the user ID is 0, skip to the next router
  if (req.params.id === '0') next('route')
  // otherwise pass control to the next middleware function in this stack
  else next()
}, (req, res, next) => {
  // render a regular page
  res.render('regular')
})

// handler for the /user/:id path, which renders a special page
router.get('/user/:id', (req, res, next) => {
  console.log(req.params.id)
  res.render('special')
})

// mount the router on the app
app.use('/', router)
```

**Error-handling middleware**
Error-handling middleware always takes four arguments. You must provide four arguments to identify it as an error-handling middleware function. Even if you don’t need to use the next object, you must specify it to maintain the signature. Otherwise, the next object will be interpreted as regular middleware and will fail to handle errors.
Define error-handling middleware functions in the same way as other middleware functions, except with four arguments instead of three, specifically with the signature (err, req, res, next)):
```
app.use((err, req, res, next) => {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

**Built-in middleware**
- [express.static](https://expressjs.com/en/4x/api.html#express.static) serves static assets such as HTML files, images, and so on.
- [express.json](https://expressjs.com/en/4x/api.html#express.json) parses incoming requests with JSON payloads. NOTE: Available with Express 4.16.0+
- [express.urlencoded](https://expressjs.com/en/4x/api.html#express.urlencoded) parses incoming requests with URL-encoded payloads. NOTE: Available with Express 4.16.0+


**Third-party middleware**
Use third-party middleware to add functionality to Express apps.
Install the Node.js module for the required functionality, then load it in your app at the application level or at the router level.
The following example illustrates installing and loading the cookie-parsing middleware function cookie-parser.
```
// $ npm install cookie-parser

const express = require('express')
const app = express()
const cookieParser = require('cookie-parser')

// load the cookie-parsing middleware
app.use(cookieParser())
```

#### [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html)
#### [Production best practices: performance and reliability](https://expressjs.com/en/advanced/best-practice-performance.html)
**Things to do in your code (the dev part):**
- Use gzip compression
- Don’t use synchronous functions
- Do logging correctly
- Handle exceptions properly

**Things to do in your environment / setup (the ops part):**
- Set NODE_ENV to “production”
- Ensure your app automatically restarts
- Run your app in a cluster
- Cache request results
- Use a load balancer
- Use a reverse proxy

#### [Production Best Practices: Security](https://expressjs.com/en/advanced/best-practice-security.html)
Security best practices for Express applications in production include:
- Don’t use deprecated or vulnerable versions of Express
- Use TLS
- Use Helmet
- Use cookies securely
- Prevent brute-force attacks against authorization
- Ensure your dependencies are secure
- Avoid other known vulnerabilities
- Additional considerations
   
   Here are some further recommendations from the excellent [Node.js Security Checklist](https://blog.risingstack.com/node-js-security-checklist/). Refer to that blog post for all the details on these recommendations:
  - Use [csurf](https://www.npmjs.com/package/csurf) middleware to protect against cross-site request forgery (CSRF).
  - Always filter and sanitize user input to protect against cross-site scripting (XSS) and command injection attacks.
  - Defend against SQL injection attacks by using parameterized queries or prepared statements.
  - Use the open-source [sqlmap](http://sqlmap.org/) tool to detect SQL injection vulnerabilities in your app.
  - Use the [nmap](https://expressjs.com/en/advanced/best-practice-security.html#:~:text=Use%20the-,nmap,-and%20sslyze%20tools) and [sslyze](https://github.com/nabla-c0d3/sslyze) tools to test the configuration of your SSL ciphers, keys, and renegotiation as well as the validity of your certificate.
  - Use [safe-regex](https://www.npmjs.com/package/safe-regex) to ensure your regular expressions are not susceptible to [regular expression denial of service](https://www.owasp.org/index.php/Regular_expression_Denial_of_Service_-_ReDoS) attacks. 

#### [Health Checks and Graceful Shutdown](https://expressjs.com/en/advanced/healthcheck-graceful-shutdown.html)
#### [Process managers for Express apps](https://expressjs.com/en/advanced/pm.html)


