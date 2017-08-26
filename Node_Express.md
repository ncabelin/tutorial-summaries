# Node, NPM and Express tuts

### Basic Node Concepts
* Run `npm init` to create a package.json
* Install express and save in package.json
`npm install express --save`
* Modules - don't affect other js files, self contained
```
// greet.js
var greets = function() {
  console.log('greeting');
}
module.exports = greets;

// app.js
var greet = require('./public/greet');
// you can use the greet function now
greet();
```

### Express Basics
1. Install 'express' through npm
2. 'require' it :
```
var express = require('express');
var app = express();
```
3. Optional : Add MIDDLEWARE
4. Add ROUTES :
```
app.get('/', function(req, res) {
    res.send('whatever');
  });
```
5. Add LISTENER :
```
// port and ip can be set as the following, for c9
var port = process.env.PORT || 8080;
var ip = process.env.IP;
app.listen(port, ip, function() {
    console.log('Server started');
  });
```

### USING A TEMPLATING ENGINE ( EJS )
* HOW TO SET UP STATIC ASSETS e.g. 'css'
```
<link rel="stylesheet" href="app.css" />

// put this after 'require express'
// to be able to use /folderName (must create it first)
app.use(express.static('folderName'));
```

* HOW TO SET UP EJS
1. Create a 'VIEWS' folder at the project root
2. NPM install ejs, then setup EJS in the main js file.
```
app.set('view engine', 'ejs');
```
3. Create your 'ejs' files in the VIEWS folder
4. Render your .ejs files, ex. `res.render('file.ejs')`

## HOW TO CREATE CONTROL FLOWS / LOOPS in EJS
* How to use Control Flows in EJS :
```
<% if (statement) %>
```
* Loops in EJS :
```
<%= jsRender %> with html if to be evaluated
otherwise <% jsLogic %> for each line of code
```

### HOW TO CREATE PARTIALS (BOILERPLATE)
1. Make directory 'PARTIALS' within 'VIEWS folder
2. Make files 'header.ejs','footer.ejs'
3. Put `<% include partials/header.ejs %>` to insert partials

### HOW TO RECEIVE A FORM SUBMISSION AS Js OBJECT
* Use Body-parser
* `$ npm install body-parser --save`
```
var bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({ extended: true}));
app.post('/postURL', function(req,res) {
  var whatever = req.body.<form-variable>;
});

// HTML
<form action="/friends" method="POST">
  <input type="text" name="<form-variable>" placeholder="name" />
  <button type="submit">Submit</button>
</form>
```

## How to handle parameters
```
app.get('/cities/:name', function(req, res) {
  var cityName = req.params.name;
});
```

# MIDDLEWARE
### How to apply a function to all parameters that are a certain way
```
// this applies the same function on all instances of name as parameter
app.param('name', function(req, res, next) {
  req.cityName = parseCityName(req.params.name);
  next();
});
```

### How to shorten and use external module files for ROUTERs
```
var router = express.Router();
router.route('/').get(function(req, res) {

  }).post(function(req, res) {

  });

app.use('/cities', router) // to use the router
app.all() // for all HTTP verbs
```

# REQUEST
* How to use an external API for your express app
```
// npm install request
var request = require('request');
request('http://www.api.com', function(err, response, body) {
  if (error) {
    // do error stuff
  } else {
    if (response.statusCode === 200) {
      var data = JSON.parse(body); // convert to JS object
    }
  }
});
```

## PUT AND DELETE REQUESTS
* PUT methods are not possible by default in web browsers
1. so we need to append `?_method=PUT` to the POST html form on the action string.
2. `npm install method-override`
3. `app.use(methodOverride('_method'));`

## FOLDER STRUCTURE
```
server.js
package.json
.env
.gitignore
/app
  /models
    todo.js - declare mongoose models
  route.js -
/public
  <front-end files>
```
