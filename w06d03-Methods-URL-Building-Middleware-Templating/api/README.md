# API

## Release 0
Create an API with the following features
- supports all CRUD operations of an object: Books
- No authentication needed
- Edit, Replace should also be supported
- No frontend needed. Write tests, a lot of them.
- You can use postman for manually testing the API


## Release 1
Above API should be seperated and should be used as a middleware.
i.e. `app.use(api())` should work. Or you can think of other strategy.

## Release 2
Let's create html views now.

The extension at the end of url should decide whether to respond with json or html data.
e.g.
`example.com/books/algorithms-cormen.json` vs `example.com/books/algorithms-cormen.html`
The `.json` should produce a json data from API whereas `.html` should return a html page that contains this book details


## Hints
- Quickly setup mongo and start cracking the API building methods
- You should setup layout engine for producing views
