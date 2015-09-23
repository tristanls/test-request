# test-request

Helper to test HTTP requests.

## Why?

I found myself writing a lot of boilerplate when testing interactions with HTTP servers. `test-request` is that boilerplate extracted into a module so I can easily use it in my projects.

## What?

`test-request` provides boilerplate to register `req.on('response')` callback and automatically closes the servers being tested when response 'end' event is triggered.

## Usage

Examples below use [nodeunit](https://github.com/caolan/nodeunit) test framework convention, but `nodeunit` is not a dependency.

Single server being tested:

```javascript
var http = require('http');
var testRequest = require('test-request');

var tests = module.exports = {};

// using nodeunit here
tests["single server usage"] = function(test)
{
    var server = http.createServer(SERVER_OPTIONS);
    /* server setup and .listen() code */
    var req = http.request(REQUEST_OPTIONS);
    testRequest(test.done, server, req, function(res)
    {
        test.equal(res.statusCode, 200, 'expected 200 status code');
    });
    req.end();
}
```

Multiple servers being tested:

```javascript
var http = require('http');
var testRequest = require('test-request');

var tests = module.exports = {};

tests["multiple server usage"] = function(test)
{
    var proxy = http.createServer(PROXY_OPTIONS);
    var server = http.createServer(SERVER_OPTIONS);
    /* proxy and server setup and .listen() code */
    var req = http.request(REQUEST_OPTIONS);
    testRequest(test.done, [server, proxy], req, function(res)
    {
        test.equal(res.statusCode, 200, 'expected 200 status code');
    });
    req.end();
}
```
