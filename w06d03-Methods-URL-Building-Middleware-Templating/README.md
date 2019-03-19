# Learning Competencies

- Methods in Express
- URL Building
- Middleware in Express
- Templating Engines

# Introduction

## Express Methods

- app.all(path, callback [, callback ...])

```javascript
The following callback is executed for requests to /secret whether using GET, POST, PUT, DELETE, or any other HTTP request method:

app.all('/secret', function (req, res, next) {
  console.log('Accessing the secret section ...')
  next() // pass control to the next handler
});
```

The `app.all()` method is useful for mapping “global” logic for specific path prefixes or arbitrary matches. For example, if you put the following at the top of all other route definitions, it requires that all routes from that point on require authentication, and automatically load a user. Keep in mind that these callbacks do not have to act as end-points: `loadUser` can perform a task, then call `next()` to continue matching subsequent routes.

- app.delete(path, callback [, callback ...])

Routes HTTP DELETE requests to the specified path with the specified callback functions.

```javascript
app.delete('/', function (req, res) {
  res.send('DELETE request to homepage');
});
```

- app.engine(ext, callback)

Registers the given template engine callback as ext.

By default, Express will require() the engine based on the file extension. For example, if you try to render a “foo.pug” file, Express invokes the following internally, and caches the require() on subsequent calls to increase performance.

```javascript
app.engine('pug', require('pug').__express);
```

Use this method for engines that do not provide .__express out of the box, or if you wish to “map” a different extension to the template engine.

For example, to map the EJS template engine to “.html” files:

```javascript
app.engine('html', require('ejs').renderFile);
```

- app.get(path, callback [, callback ...])

Routes HTTP GET requests to the specified path with the specified callback functions.

```javascript
app.get('/', function (req, res) {
  res.send('GET request to homepage');
});
```

- app.listen(path, [callback])

Starts a UNIX socket and listens for connections on the given path. This method is identical to Node’s http.Server.listen().

```javascript
var express = require('express');
var app = express();
app.listen('/tmp/sock');
```

- app.listen(port, [hostname], [backlog], [callback])

Binds and listens for connections on the specified host and port. This method is identical to Node’s http.Server.listen().

```javascript
var express = require('express');
var app = express();
app.listen(3000);
```

The app returned by express() is in fact a JavaScript Function, designed to be passed to Node’s HTTP servers as a callback to handle requests. This makes it easy to provide both HTTP and HTTPS versions of your app with the same code base, as the app does not inherit from these (it is simply a callback):

```javascript
var express = require('express');
var https = require('https');
var http = require('http');
var app = express();

http.createServer(app).listen(80);
https.createServer(options, app).listen(443);
```
The app.listen() method returns an http.Server object and (for HTTP) is a convenience method for the following:

```javascript
app.listen = function() {
  var server = http.createServer(this);
  return server.listen.apply(server, arguments);
};
```

- app.METHOD(path, callback [, callback ...])

Routes an HTTP request, where METHOD is the HTTP method of the request, such as GET, PUT, POST, and so on, in lowercase. Thus, the actual methods are app.get(), app.post(), app.put(), and so on. See Routing methods below for the complete list.

Express supports the following routing methods corresponding to the HTTP methods of the same names:

```
checkout
copy
delete
get
head
lock
merge
mkactivity
mkcol
move
m-search
notify
options
patch
post
purge
put
report
search
subscribe
trace
unlock
unsubscribe
```

Check out more express methods [here](https://expressjs.com/en/api.html#app.disable).....


## URL Building

We can now define routes, but those are static or fixed. To use the dynamic routes, we SHOULD provide different types of routes. Using dynamic routes allows us to pass parameters and process based on them.

Here is an example of a dynamic route −

```javascript
var express = require('express');
var app = express();

app.get('/:id', function(req, res){
   res.send('The id you specified is ' + req.params.id);
});
app.listen(3000);
```

To test this go to http://localhost:3000/123. The following response will be displayed.

You can replace '123' in the URL with anything else and the change will reflect in the response. A more complex example of the above is −

```javascript
var app = express();

app.get('/things/:name/:id', function(req, res) {
   res.send('id: ' + req.params.id + ' and name: ' + req.params.name);
});
app.listen(3000);
```

To test the above code, go to http://localhost:3000/things/tutorialspoint/12345.

You can use the req.params object to access all the parameters you pass in the url. Note that the above 2 are different paths. They will never overlap. Also if you want to execute code when you get '/things' then you need to define it separately.

### Pattern Matched Routes

You can also use regex to restrict URL parameter matching. Let us assume you need the id to be a 5-digit long number. You can use the following route definition −

```javascript
var express = require('express');
var app = express();

app.get('/things/:id([0-9]{5})', function(req, res){
   res.send('id: ' + req.params.id);
});

app.listen(3000);
```

Note that this will only match the requests that have a 5-digit long id. You can use more complex regexes to match/validate your routes. If none of your routes match the request, you'll get a "Cannot GET <your-request-route>" message as response. This message be replaced by a 404 not found page using this simple route −

```javascript
var express = require('express');
var app = express();

//Other routes here
app.get('*', function(req, res){
   res.send('Sorry, this is an invalid URL.');
});
app.listen(3000);
```

Important − This should be placed after all your routes, as Express matches routes from start to end of the index.js file, including the external routers you required.

## Middleware

Middleware functions are functions that have access to the request object (req), the response object (res), and the next middleware function in the application’s request-response cycle. The next middleware function is commonly denoted by a variable named next.


Middleware functions can perform the following tasks:

- Execute any code.
- Make changes to the request and the response objects.
- End the request-response cycle.
- Call the next middleware function in the stack.

If the current middleware function does not end the request-response cycle, it must call next() to pass control to the next middleware function. Otherwise, the request will be left hanging.

# why's and how's??

Middleware is code that gets run in between the request and the response. We've seen some examples of middleware already; ``body-parser`` and ``method-override`` are both examples of middleware. But we can write our own middleware as well! We will also be using our own custom middleware to configure the express router and handle errors.

To include middleware we use the app.use function. Here are two examples:
```javascript
// on every single request, run the following
app.use(function(req, res, next){
    console.log('Middleware just ran!')
    next()
})
// on every single request to anything that starts with /users, run the following
app.use('/users', function(req, res, next){
    console.log('Middleware just ran on a user route!')
    next()
})
```
### Error Handling
Another tool that we can add to our applications is an easier way to manage errors (especially in production). This middleware allows errors to be built up and finally sent to an errors page. This pattern is also quite useful when you have lots of different levels of nesting and there's potential for errors to be uncaught.
```javascript
// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handlers

// development error handler
// will print stacktrace - make sure you have a file called views/error.pug to render this error message!
if (app.get('env') === 'development') {
  app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
      message: err.message,
      error: err
    });
  });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
  res.status(err.status || 500);
  res.render('error', {
    message: err.message,
    error: {}
  });
});
```
You can read more about why it is so valuable to always have the next parameter and use it [here](https://derickbailey.com/2014/09/06/proper-error-handling-in-expressjs-route-handlers/) and [here](https://expressjs.com/en/guide/error-handling.html).

### ``morgan``
Before we continue on, let's examine a useful module that will help log out requests and responses to the terminal. This module is called morgan, and can be very helpful when you're trying to debug your application.
```
mkdir another_express_app && cd another_express_app
touch app.js
npm init -y
npm install --save express pug body-parser morgan
```
Here is what our app.js might look like with all our middleware set up!
```javascript
var express = require("express");
var app = express();
var morgan = require("morgan");
var bodyParser = require("body-parser");

app.set("view engine", "pug");
app.use(express.static(__dirname + "/public"));
app.use(morgan("tiny"))
app.use(bodyParser.urlencoded({extended:true}));

app.get("/", function(req, res, next){
  res.render("index");
});

// catch 404 and forward to error handler
app.use(function(req, res, next) {
  var err = new Error('Not Found');
  err.status = 404;
  next(err);
});

// error handlers

// development error handler
// will print stacktrace
if (app.get('env') === 'development') {
  app.use(function(err, req, res, next) {
    res.status(err.status || 500);
    res.render('error', {
      message: err.message,
      error: err
    });
  });
}

// production error handler
// no stacktraces leaked to user
app.use(function(err, req, res, next) {
  res.status(err.status || 500);
  res.render('error', {
    message: err.message,
    error: {}
  });
});


app.listen(3000, function(){
  console.log("Server is listening on port 3000");
});
```
### Generator
So far we have been generating our express applications starting with a brand new folder, installing packages and building applications from scratch. There is a faster tool for the job called express-generator, which you can install using :

    npm install -g express-generator.
 We will not be making use of this module since it's valuable to learn how to create these applications from scratch, but it is a commonly used tool for generating applications quickly. You can read more about it [here](https://expressjs.com/en/starter/generator.html).

### Debugging Node
One easy way to debug node application is to simply ``console.log`` values and see what the error could be. Unfortunately, this is quite time consuming so there is a better tool called ``locus``, which you can install using

      npm install --save locus
 It helps to pause your code at a certain line so that you can then go to the terminal and examine variables and write JavaScript. Once you have installed the module, place the line eval(``require("locus")``) in your application and when your code reaches that point, you can debug in the terminal.


## Templating

We previously saw the core HTTP module which allows us to make requests and issue responses, but we also mentioned that using the HTTP module requires a fair amount code and complexity. Thankfully `express` abstracts away much of this complexity. So let's get started with it! In the terminal, let's type the following:
```
# create a folder and cd into it
mkdir first_express_app && cd first_express_app
# create a file called app.js (doing this before npm init is important, we will see why later!)
touch app.js
# create a package json file
npm init -y
# install the express module and save the module name and version to our package.json so that when we work with other developers they can easily install all of our dependencies
npm install --save express
```
Inside of our `app.js` let's add the following:
```js
// require the express module
var express = require("express");
// create an object from the express function which we contains methods for making requests and starting the server
var app = express();

// create a route for a GET request to '/' - when that route is reached, run a function
app.get('/', function(request, response){
    // inside of this callback we have two large objects, request and response
        // request - can contain data about the request (query string, url parameters, form data)
        // response - contains useful methods for determining how to respond (with html, text, json, etc.)
    // let's respond by sending the text Hello World!
    response.send('Hello World!');
})

// let's tell our server to listen on port 3000 and when the server starts, run a callback function that console.log's a message
app.listen(3000, function(){
    console.log("The server has started on port 3000. Head to localhost:3000 in the browser and see what's there!");
});
```
Now let's start the server using node `app.js` and head over to `localhost:3000`. To stop the server press `control + c`. Remember that when the server is running, you will not be able to type commands in that tab in the terminal. If you want to run other command line commands, open a different tab.

### `nodemon`
Nodemon is very useful for restarting the server when it goes down so that you don't have to manually restart it yourself. To install `nodemon` type the following command anywhere in the terminal `npm install -g nodemon` if you are getting errors when installing this, you can use `sudo npm install -g nodemon` and type in your password and press enter to install it. (If you need to use `sudo` you've got a permissions issue with `npm`; follow [these](https://docs.npmjs.com/getting-started/fixing-npm-permissions) instructions to fix the problem.) To start the server now you can use `nodemon app.js`.

### url parameters
So far we have just seen how to build static routes, but what makes server-side programming so powerful is that we can create dynamic responses based on the type of request. This is how applications can have one single route that provides many different kinds of responses depending on the data that is passed in the URL. Dynamic values to be passed in the URL are called "URL parameters" and are stored in the `params` object which exists in `request` object given to us in our callback functions.

To specify that a part of a URL will be a "parameter", we add the `:` character and then give our URL parameter a name. This name will be the key in the `params` object. Let's see what that looks like below. It is also important to note that all of our URL parameters are strings! Let's take a look at an `app.js` file that has a route with URL parameters (notice the `:`)
```js
// same pattern as above, require express, invoke the express function
var express = require("express");
var app = express();

// same as above, listen for a GET request
app.get('/', function(request, response){
    response.send('Hello World!')
})

// when a request comes in to /instructors/ANYTHING
app.get('/instructor/:first_name', function(request,response){
    // let's capture the "dynamic" part of the URL, which we are called "first_name". The name that we give to this dynamic part of the URL will become a key in the params object which exists on the request object.

    // let's send back some text with whatever data came in the URL!
    response.send(`The name of this instructor is ${request.params.first_name}`)
});

app.listen(3000, function(){
    console.log("The server has started on port 3000. Head to localhost:3000 in the browser and see what's there!")
})
```
To run this application we can either type `node app.js`, but if we need to make any changes to the file we would need to restart the server to see this change. Thankfully we have `nodemon` to help us manage starting and restarting the server when a change happens, so we can type `nodemon app.js` or just `nodemon` and our server will start!

Now if we head over to `localhost:3000/instructor/elie` and then `localhost:3000/instructor/matt` and then `localhost:3000/instructor/tim`, what do you notice? Even though we created one single route, we can now issue different responses depending on what the user has typed in the browser! This is the foundation for how to build dynamic applications (the url parameter could be a value we look up in a database to display different profiles for the same route). It is also important to note that **ALL** URL parameters are strings, so if we try to work with anything inside of `request.params`, it will always be a `string`.

### render
So far we have only seen how to respond by sending text, but when building server-side applications, we commonly want to send back HTML templates with data created on the server. If we want to simply send HTML back we can use a method on the `response` object called `sendFile`, but since we are using server-side templates, we will be using a method called `render`.

In order to do that, we need to make use of a templating engine (a tool for creating HTML that includes server side data). There are quite a few we can use with Node.js including `ejs`, but we will be using one called `pug` (formerly known as `jade`). Before we introduce the pug syntax, let's first start a new project and see what we need to include. By default, express expects a folder containing templates to be called `views`. Instead of overriding that default, we will also create a folder called `views`.
```
mkdir learn_pug && cd learn_pug
touch app.js
npm init -y
npm install --save express pug # notice we are installing pug as well!
mkdir views
touch views/index.pug
```
Let's now get started in our `app.js`:
```js
var express = require("express");
var app = express();

app.set('view engine', 'pug') // notice here we are telling express to render views using the pug templating engine.

app.get('/', function(request, response){
    response.render('index') // here we are rendering a template called index.pug inside of the views folder
})

app.listen(3000, function(){
    console.log("The server has started on port 3000. Head to localhost:3000 in the browser and see what's there!")
})
```
### Working with pug
The most important thing to understand about `pug` is that it is an indentation based syntax. To nest elements inside of each other we simply indent the inner elements.
```js
var express = require("express");
var app = express();

var colors = ["red", "green", "blue"]

app.set('view engine', 'pug') // notice here we are telling express to render views using the pug templating engine.

app.get('/', function(request, response){
    var firstName = "Elie"
    response.render('index', {name: firstName}) // here we are rendering a template called index.pug inside of the views folder. We are also passing a value to the template called "name". Inside of the template, the value of that variable will be the value of the firstName variable, which is "Elie"
})

app.get('/colors', function(request, response){
    // {colors} is ES2015 object shorthand notation for {colors: colors}
    response.render('data', {colors}) // here we are rendering a template called data.pug inside of the views folder. We are also passing a value to the template called "colors". Inside of the template, the value of that variable will be the value of the colors variables, which is an array.
})

app.listen(3000, function(){
    console.log("The server has started on port 3000. Head to localhost:3000 in the browser and see what's there!")
})
```
To evaluate variables inside of our templates, we will use the Jade `#{variable}` syntax. When you do string concatenation with Jade - use the ES2015 string templates with the `${javascript to interpolate}` syntax. Here is what our `index.pug` file might look like:
```
h1 Hello #{name}
```
And our `colors.pug` - notice we are using the Jade syntax `#{}` for variables and ES2015 `${}` for string concatentation/interpolation.
```
h1 Here are the colors!
each color in colors
    p #{color}
    a(href=`/colors/${color}`) Learn more about this color
```
### Express and pug
Now in our `views/index.pug` file let's place the following:

```html
<!DOCTYPE html>
html(lang="en")
head
    meta(charset="UTF-8")
    title Document
body
    h1 "Hello World!"
    p Outer Paragraph
        .foo A div with a class of foo
    h2 Let's do some math! #{5+5}
```
### Template inheritance with pug
One very useful feature that `pug` gives us is called *template inheritance*. What this means is that we can use the same template in all of our files and only have to define it once! We can include templates using the `extends` keyword. We also need to tell our template where to inject HTML from other files; we do this with the `block` keyword.

Let's look at an example. In our views folder, let's create a file called `base.pug`.

```html
<!DOCTYPE html>
html(lang="en")
head
    meta(charset="UTF-8")
    title Document
body
    block content
```
Next, let's create a `hello.pug`, extend our `base.pug` file, and include our own content! We just have to make sure that the content specific to `hello.pug` is indented inside of `block content`

```
extends base.pug

block content
    h1 Hello World!
```
For more examples of the features available to you in pug, take a look at the [documentation](https://pugjs.org/).

### css/js using `express.static`
Before we continue with routing, let's examine how to tell our server how to serve static assets (css/javascript/images). To do this we create a folder called `public` in the root directory. We can then add the following in our `app.js`:

```js
var express = require("express");
var app = express();

app.set("view engine", "pug");
app.use(express.static(__dirname + "/public")); // we are using the express.static middleware and specifying a path for static files to be found (__dirname is a variable that we can use to refer to the directory name of the current module)

// even though we were previously using request and response as names of parameters, you will very commonly see these as req and res.
app.get("/", function(req, res, next){
  res.render("index");
});

app.listen(3000, function(){
  console.log("Server is listening on port 3000");
});
```