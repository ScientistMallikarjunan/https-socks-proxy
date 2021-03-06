HTTPS SOCKS PROXY Agent
================
### A SOCKS proxy `http.Agent` implementation for HTTP and HTTPS

This module provides an `http.Agent` implementation that connects to a
specified SOCKS proxy server, and can be used with the built-in `http`
,`https` and `node-fetch` modules.

It can also be used in conjunction with the `ws` module to establish a WebSocket
connection over a SOCKS proxy. See the "Examples" section below.

Installation
------------

Install with `npm`:

``` bash
$ npm install https-socks-proxy
```


Examples
--------

#### TypeScript example

```ts
import https from 'https';
import { SocksProxyAgent } from 'https-socks-proxy';

const info = {
	host: 'br41.nordvpn.com',
	userId: 'your-name@gmail.com',
	password: 'abcdef12345124'
};
const agent = new SocksProxyAgent(info);

https.get('https://jsonip.org', { agent }, (res) => {
	console.log(res.headers);
	res.pipe(process.stdout);
});
```

#### `http` module example

```js
var url = require('url');
var http = require('http');
var SocksProxyAgent = require('https-socks-proxy');

// SOCKS proxy to connect to
var proxy = process.env.socks_proxy || 'socks://127.0.0.1:1080';
console.log('using proxy server %j', proxy);

// HTTP endpoint for the proxy to connect to
var endpoint = process.argv[2] || 'http://nodejs.org/api/';
console.log('attempting to GET %j', endpoint);
var opts = url.parse(endpoint);

// create an instance of the `SocksProxyAgent` class with the proxy server information
var agent = new SocksProxyAgent(proxy);
opts.agent = agent;

http.get(opts, function (res) {
	console.log('"response" event!', res.headers);
	res.pipe(process.stdout);
});
```

#### `https` module example

```js
var url = require('url');
var https = require('https');
var SocksProxyAgent = require('https-socks-proxy');

// SOCKS proxy to connect to
var proxy = process.env.socks_proxy || 'socks://127.0.0.1:1080';
console.log('using proxy server %j', proxy);

// HTTP endpoint for the proxy to connect to
var endpoint = process.argv[2] || 'https://encrypted.google.com/';
console.log('attempting to GET %j', endpoint);
var opts = url.parse(endpoint);

// create an instance of the `SocksProxyAgent` class with the proxy server information
var agent = new SocksProxyAgent(proxy);
opts.agent = agent;

//If you want HTTPS communication with CA enabled, then pass the following options
// rejectUnauthorized : true / false
// ca : [List of CA]

https.get(opts, function (res) {
	console.log('"response" event!', res.headers);
	res.pipe(process.stdout);
});
```

#### `ws` WebSocket connection example

``` js
var WebSocket = require('ws');
var SocksProxyAgent = require('https-socks-proxy');

// SOCKS proxy to connect to
var proxy = process.env.socks_proxy || 'socks://127.0.0.1:1080';
console.log('using proxy server %j', proxy);

// WebSocket endpoint for the proxy to connect to
var endpoint = process.argv[2] || 'ws://echo.websocket.org';
console.log('attempting to connect to WebSocket %j', endpoint);

// create an instance of the `SocksProxyAgent` class with the proxy server information
var agent = new SocksProxyAgent(proxy);

// initiate the WebSocket connection
var socket = new WebSocket(endpoint, { agent: agent });

socket.on('open', function () {
	console.log('"open" event!');
	socket.send('hello world');
});

socket.on('message', function (data, flags) {
	console.log('"message" event! %j %j', data, flags);
	socket.close();
});
```

License
-------

(The MIT License)

Copyright (c) 2020 Mallikarjunan M <https://github.com/ScientistMallikarjunan>

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
