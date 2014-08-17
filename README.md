#Universal HTTP(S) Request for Catberry[![Build Status](https://travis-ci.org/catberry/catberry-uhr.png?branch=master)](https://travis-ci.org/catberry/catberry-uhr)
[![NPM](https://nodei.co/npm/catberry-uhr.png)](https://nodei.co/npm/catberry-uhr/)

##Description
Catberry's modules run both at server and in browser and it is very useful to 
have universal HTTP(S) request implementation.

It has one interface and different implementations at server and in browser.

At server it uses node's [http.request](http://nodejs.org/api/http.html#http_event_request) 
or [https.request](http://nodejs.org/api/https.html#https_https_request_options_callback)
(depend on specified protocol in URL).
At browser it uses native [XmlHttpRequest](https://developer.mozilla.org/ru/docs/Web/API/XMLHttpRequest).

This module was developed using [HTTP/1.1v2 RFC 2616](http://www.w3.org/Protocols/rfc2616).

It supports:

 * `gzip` and `deflate` request/response content encodings
 * `application/json` and `application/x-www-form-urlencoded` 
 request/response content types
 * Request timeout
 * Auto stringify/parse request/response data
 * HTTP/HTTPS
 * Any additional HTTP headers you set

This Catberry service has following methods.

```javascript
/**
 * Does GET request to HTTP server.
 * @param {string} url URL to request.
 * @param {Object} options Object with options.
 * @param {Function<Error, Object, string>?} callback Callback on finish
 * with error, status object and data.
 */
UHRBase.prototype.get = function (url, options, callback) { }

/**
 * Does POST request to HTTP server.
 * @param {string} url URL to request.
 * @param {Object} options Request options.
 * @param {Function<Error, Object, string>?} callback Callback on finish
 * with error, status object and data.
 */
UHRBase.prototype.post = function (url, options, callback) { }

/**
 * Does PUT request to HTTP server.
 * @param {string} url URL to request.
 * @param {Object} options Object with options.
 * @param {Function<Error, Object, string>?} callback Callback on finish
 * with error, status object and data.
 */
UHRBase.prototype.put = function (url, options, callback) { }

/**
 * Does DELETE request to HTTP server.
 * @param {string} url URL to request.
 * @param {Object} options Object with options.
 * @param {Function<Error, Object, string>?} callback Callback on finish
 * with error, status object and data.
 */
UHRBase.prototype.delete = function (url, options, callback) { }

/**
 * Does request with specified parameters.
 * @param {Object} parameters Request parameters.
 * @param {Function<Error, Object, string>?} callback Callback on finish
 * with error, status object and data.
 */
UHRBase.prototype.request = function (parameters, callback) { }
```

##Request options example

```javascript
{
	method: 'GET',
	timeout: 30000,
	headers: {
		Cookie: 'name=value'
	},
	data: {
		parameter: 'value'
	}
}
```

In case you do `GET`/`DELETE` request `data` will be passed as query string 
otherwise it will be passed as JSON via request stream. Also if you put
something to `data` field and use `application/x-www-form-urlencoded` then this
data will be automatically [encoded](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent).

##Callback
In callback you will receive:

* `Error` (if transport level error was happened)
* Status object with HTTP status code, status text and response headers
* Response body as plain text or object 
(depends on `Content-Type` in response headers)

Status object looks like this:

```javascript
{
	code: 200,
	text: 'OK',
	headers: {
		'Cache-Control': 'no-cache',
        'Content-Length': '1',
        'Content-Type': 'text/html; charset=utf-8',
        'Date': 'Tue, 08 Apr 2014 05:16:19 GMT'
	}
}
```

##Usage
If you are using [Catberry Framework](https://github.com/catberry/catberry)
it is already included and registered in [Service Locator](https://github.com/catberry/catberry/blob/master/docs/services/service-locator.md).

You can just inject `$uhr` into your module and use like this:

```javascript
function Module($uhr) {
	this._uhr = $uhr;
}

Module.prototype.render(placeholderName, callback) {
	var options = {
		timeout: 3000,
		data: {
			username: 'some'
		},
		headers: {
			Authorization: 'Bearer somecrazytoken'
		}
	};
	this._uhr.get('http://localhost/api/user', options,
    		function (error, status, data) {
    			if (error) {
    				callback(error);
    				return;
    			}
    			callback(null, data);
    		});
	...
}
```

##Contribution
If you have found a bug, please create pull request with [mocha](https://www.npmjs.org/package/mocha) 
unit-test which reproduces it or describe all details in issue if you can not 
implement test. If you want to propose some improvements just create issue or 
pull request but please do not forget to use `npm test` to be sure that your 
code is awesome.

All changes should satisfy this [Code Style Guide](https://github.com/catberry/catberry/blob/master/docs/code-style-guide.md).

Also your changes should be covered by unit tests using [mocha](https://www.npmjs.org/package/mocha).

Denis Rechkunov <denis.rechkunov@gmail.com>