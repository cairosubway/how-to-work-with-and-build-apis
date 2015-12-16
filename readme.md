# How to Work With and Build APIs

![](http://washingtontechnology.org/wp-content/uploads/2014/11/General_Assembly_logo.png)

## Introduction to APIs

### Installfest

Tools needed for today’s class:
- git
- mongodb
- node and npm

### What's an API?

- API is an acronym for Application Program Interface
- An API is a service that provides raw data for public use
- Application programming interface technically applies to all of software design
- Term API evolved to define specifically URLs that allow the user to access raw data
- APIs publish data for public use
- As third-party software developers, we can access an organization’s API and use their data in our applications

A couple examples - stock quotes
http://dev.markitondemand.com/MODApis/Api/Quote/json?symbol=AAPL
http://dev.markitondemand.com/MODApis/Api/Quote/json?symbol=GOOGL

### Types of serialized data

#### JSON
JavaScript Object Notation
Universal standard for serializing native data structures for transmission
Lightweight
Easy to read
JSON needs to be parsed before we can use it

#### XML
eXstensible Markup Language
Granddaddy of serialized data formats
Based on HTML
Cumbersome to parse
Frequent legacy use
You’ll probably favor using a JSON API, if it’s available

Same API, different formats
Many APIs publish data in different formats, such as:
http://dev.markitondemand.com/Api/Quote/json?symbol=AAPL
http://dev.markitondemand.com/Api/Quote/xml?symbol=AAPL

### We do: A close look at an API request with Postman
Let’s make a basic HTTP request to an API
We can do this in the browser, but let’s use Postman to take a detailed look
What is Postman?
A Chrome extension for making HTTP requests 

### You do: Talk with the Postman
- Download Postman
- Go to https://www.getpostman.com/
- Open the app
- Type the URL of an API call
- Ensure the method is GET
- Click Send

### How do we identify resources?

- Uniform Resource Identifier (URI) is a string of characters that identifies the name of a resource
- The most common URI is the Universal Resource Locator (URL) or web address
- The URL goes a step farther than the URI and specifies the means of accessing the resource (HTTP)
- There are two primary uses for APIs

### Programmatically accessing data

#### What is HTTP?
- HTTP is an acronym for Hypertext Transfer Protocol
- Hypertext is text displayed on an electronic device with links to other text
- Underlying protocol for data exchange on the World Wide Web

#### How does HTTP work?	
- HTTP is a client-server protocol
- A client-server protocol divides tasks between the server, the resource provider, and the client, the resource requestor
- HTTP is a request-response protocol
- The client requests data and the server responds to the request
- HTTP works through request methods, also called HTTP verbs
- HTTP verbs describe the action you want to do on the desired resource

#### HTTP verbs

| Method | Description |
| --- | --- |
| GET | requests a representation of the specified resource, only retrieves data |
| POST | requests that the server accept the data enclosed in the request as a new subordinate of the resource the URI identifies |
| PUT | requests the enclosed entity be stored under the supplied URI |
| DELETE | deletes the selected resource |

- HTTP is stateless
- HTTP treats each request as an independent transaction that is unrelated to any previous request
- The communication consists of independent pairs of request and response
- You can understand each request message in isolation

#### What is REST?

- REST stands for Representational State Transfer
- REST is an architectural style for designing networked applications
- RESTful applications use HTTP requests to post data, read data and delete data
- The World Wide Web is itself a gigantic RESTful application

#### How does REST work?
- REST tries to keep things as simple as possible
- Uses HTTP request methods as a resource interface
- REST uses HTTP methods to perform create, read, update, and delete (CRUD)

| Method | CRUD Action |
| --- | --- |
| GET | Read |
| POST | Create |
| PUT | Update|
| DELETE | Delete |

### 3rd party authentication

```
// TODO
```

## Working with APIs

Let's build a weather app CLI that tells us the weather given any zipcode.

[Starter Code](https://github.com/ga-dc/node-cli-weather) | [Solution Code](https://github.com/ga-dc/node-cli-weather/tree/solution)

### Initial Setup

    $ git clone https://github.com/ga-dc/node-cli-weather
    $ cd node-cli-weather
    $ npm install

- Visit http://www.wunderground.com/weather/api/d/login.html and create a free account.
- Click "Explore My Options"
- Click "Purchase Key"
- Visit http://www.wunderground.com/weather/api/d/docs and click "Show Response"

```js
// env.js
module.exports = {
  wuApiKey: "Your API Key Here"
}
```

### Looking up an area by zipcode

```js
// app.js

var env = require("./env");
var zipcode = 20002;
var apiUrl = "http://api.wunderground.com/api/" + env.wuApiKey;
var url = apiUrl + "/geolookup/q/" + zipcode + ".json";

console.log(url);
```

View this url in the browser.

### Request that json from node

```js
// app.js
...
var request = require("request");
request(url, function(err, response, body){
  console.log(response);
})
```

### Get the conditions API endpoint

```js
// app.js
...
request(url, function(err, response, body){
  var location = JSON.parse(body).location.requesturl;
  var url = apiUrl + "/conditions/q/"+location+".json";
  console.log(url);
})
```

View this url in the browser.

### Request that json from node

```js
// app.js
...
var apiUrl ="http://api.wunderground.com/api/"+env.wuApiKey;
var url = apiUrl + "/geolookup/q/"+zipcode+".json";
request(url, function(err,res,body){
  var location = JSON.parse(body).location.requesturl;
  var url = apiUrl + "/conditions/q/"+location+".json";
  request(url, function(err,res,body){
    var observation = JSON.parse(body).current_observation
    console.log("The weather in", observation.display_location.full, "is", observation.weather)
  })
})
```

## Build Your Own API

[Starter Code](https://github.com/ga-dc/products-api) | [Solution Code](https://github.com/ga-dc/products-api/tree/solution)

### Express

[Express.js](http://expressjs.com/en/index.html) is a web framework for node js.

    $ mkdir etsy-clone
    $ cd etsy-clone
    $ npm init
    $ npm install --save express

```js
// index.js

var express = require("express");
var app = express();
app.get("/", function(req, res){
  res.send("hello world");
})
```

Next, let's hardcode some data and respond with json

```js
// index.js

var products = [
  "Lip Balm Set organic lip balm stocking stuffers Natural lip balm Pick 3",
  "Purple and Gray dreamy bird ornament - Beautiful Bird decoration in beige - Upcycled Woodland hanging"
];

app.get("/products",function(req, res){
  res.json(products);
});
```

And a show route

```js
// index.js

app.get("/products/:id", function(req, res){
  var product = products[req.params.id];
  res.json(product);
});
```

>Bonus! Try adding create and delete functionality.

### MongoDB

A record in MongoDB is a document, which is a data structure composed of field and value pairs. MongoDB documents are similar to JSON objects. The values of fields may include other documents, arrays, and arrays of documents.

