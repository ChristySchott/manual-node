<h1 align="center"> Networking </h1>

- [HTTP](#http)
- [How HTTP Requests work](#http-requests)
- [Build an HTTP server](#http-server)
- [Making HTTP requests](#http-requests)
- [Axios](#axios)
- [Websockets](#websockets)
- [HTTPS, secure connections](#secure-connections)

## <a name="http"></a> HTTP

**A detailed description of how the HTTP protocol, and the Web, work**

HTTP (Hyper Text Transfer Protocol) is one of the application protocols of TCP/IP, the suite ofprotocols that powers the Internet.

Let me fix that: it's not one of the protocols, it's the most successful and popular one, by all means.

HTTP is what makes the World Wide Web work, giving browsers a language to communicateto remote servers that host web pages.

HTTP was first standardized in 1991, as a result of the work that Tim Berners-Lee did atCERN, the European Center of Nuclear Research, since 1989.

The goal was to allow researchers to easily exchange and interlink their papers. It was meantas a way for the scientific community to work better.

Back then the internet main applications basically consisted in FTP (the File TransferProtocol), Email and Usenet (newsgroups, today almost abandoned).

In 1993 Mosaic, the first graphical web browser, was released, and things skyrocketed from there.

The Web became the killer app of the Internet.

Over time the Web and the ecosystem around it have dramatically evolved, but the basics stillremain. One example of evolution: HTTP now powers, in addition to web pages, REST APIs,one common way to programmatically access a service over the Internet.

HTTP got a minor revision in 1997 with HTTP/1.1, and in 2015 its successor, HTTP/2, wasstandardized and it's now being implemented by the major Web Servers used across the globe.

The HTTP protocol is considered insecure, just like any other protocol (SMTP, FTP..) notserved over an encrypted connection. This is why there is a big push nowadays towards usingHTTPS, which is HTTP served over TLS.

That said, the building blocks of HTTP/2 and HTTPS have their roots in HTTP, and in thisarticle I'll introduce how HTTP works.

### HTML documents

HTTP is the way web browsers like Chrome, Firefox, Edge and many others (also calledclients from here on) communicate with web servers.

The name Hyper Text Transfer Protocol derives from the need of transferring not just files, likein FTP - the "File Transfer Protocol", but hypertexts, which would be written using HTML, andthen represented graphically by the browser with a nice presentation and interactive links.

Links were the driving force that drove adoption, along with the ease of creation of new webpages.

HTTP is what transfer those hypertext files (and as we'll see also images and other file types)over the network.

### Hyperlinks

Inside a web browser, a document can point to another document using links.

A link is composed by a first part that determines the protocol and the server address, eitherthrough a domain name or an IP.

This part is not unique to HTTP, of course.

Then there's the document part. Anything appended to the address part represents thedocument path.

For example, this document address is https://flaviocopes.com/http/:
- https is the protocol.
- flaviocopes.com is the domain name that points to my server
- /http/ is the document URL relative to the server root path.

The path can be nested: https://flaviocopes.com/page/privacy/ and in this case the documentURL is /page/privacy.

The web server is responsible for interpreting the request and, once analyzed, serving thecorrect response.

### A request

What's in a request?

The first thing is the URL, which we've already seen before.

When we enter an address and press enter in our browser, under the hoods the server sendsto the correct IP address a request like this:

```
  GET /a-page
```
where /a-page is the URL you requested.

The second thing is the HTTP method (also called verb).

HTTP in the early days defined 3 of them:

- `GET`
- `POST`
- `HEAD`

and HTTP/1.1 introduced

- `PUT`
- `DELETE`
- `OPTIONS`
- `TRACE`

We'll see them in detail in a minute.

The third thing that composes a request is a set of HTTP headers.

Headers are a set of key: value pairs that are used to communicate to the server-specificinformation that is predefined, so the server can know what we mean.

I described them in detail in the HTTP request headers list.

Give that list a quick look. All of those headers are optional, except `Host`.

### HTTP methods

#### `GET`

GET is the most used method here. It's the one that's used when you type an URL in thebrowser address bar, or when you click a link.

It asks the server to send the requested resource as a response.

#### `HEAD`

HEAD is just like GET, but tells the server to not send the response body back. Just theheaders.

#### `POST`

The client uses the POST method to send data to the server. It's typically used in forms, forexample, but also when interacting with a REST API.

#### `PUT`

The PUT method is intended to create a resource at that specific URL, with the parameterspassed in the request body. Mainly used in REST APIs.

#### `DELETE`

The DELETE method is called against an URL to request deletion of that resource. Mainlyused in REST APIs.

#### `OPTIONS`

When a server receives an OPTIONS request it should send back the list of HTTP methodsallowed for that specific URL.

#### `TRACE`

Returns back to the client the request that has been received. Used for debugging ordiagnostic purposes.

### HTTP Client/Server communication

HTTP, as most of the protocols that belong to the TCP/IP suite, is a stateless protocol.

Servers have no idea what's the current state of the client. All they care about is that they getrequest and they need to fulfill them.

Any prior request is meaningless in this context, and this makes it possible for a web server tobe very fast, as there's less to process, and also it gives it bandwidth to handle a lot ofconcurrent requests.

HTTP is also very lean, and communication is very fast in terms of overhead. This contrastswith the protocols that were the most used at the time HTTP was introduced: TCP andPOP/SMTP, the mail protocols, which involve lots of handshaking and confirmations on thereceiving ends.

Graphical browsers abstract all this communication, but we'll illustrate it here for learning purposes.

A message is composed by a first line, which starts with the HTTP method, then contains theresource relative path, and the protocol version:

`GET /a-page HTTP/1.1`

After that, we need to add the HTTP request headers. As mentioned above, there are manyheaders, but the only mandatory one is `Host`:

```
GET /a-page HTTP/1.1
Host: flaviocopes.com
```

How can you test this? Using telnet. This is a command-line tool that lets us connect to anyserver and send it commands.

Open your terminal, and type `telnet flaviocopes.com 80`

This will open a terminal, that tells you

```
Trying 178.128.202.129...
Connected to flaviocopes.com.
Escape character is '^]'.
```

You are connected to the Netlify web server that powers my blog. You can now type:

```
GET /axios/ HTTP/1.1
Host: flaviocopes.com
```

and press enter on an empty line to fire the request.

The response will be:

```
HTTP/1.1 301 Moved Permanently
Cache-Control: public, max-age=0, must-revalidate
Content-Length: 46
Content-Type: text/plain
Date: Sun, 29 Jul 2018 14:07:07 GMT
Location: https://flaviocopes.com/axios/
Age: 0
Connection: keep-alive
Server: Netlify

Redirecting to https://flaviocopes.com/axios/
```

See, this is an HTTP response we got back from the server. It's a 301 Moved Permanentlyrequest. See the HTTP status codes list to know more about the status codes.

It basically tells us the resource has permanently moved to another location.

Why? Because we connected to port 80, which is the default for HTTP, but on my server I setup an automatic redirection to HTTPS.

The new location is specified in the Location HTTP response header.

There are other headers, all described in the HTTP response headers list.

In both the request and the response, an empty line separates the request header from therequest body. The request body in this case contains the string

`Redirecting to https://flaviocopes.com/axios/`

Which is 46 bytes long, as specified in the Content-Length header. It is shown in the browserwhen you open the page, while it automatically redirects you to the correct location.

In this case we're using telnet, the low-level tool that we can use to connect to any server, sowe can't have any kind of automatic redirect.

Let's do this process again, now connecting to port 443, which is the default port of the HTTPSprotocol. We can't use telnet because of the SSL handshake that must happen.

Let's keep things simple and use curl, another command-line tool. We cannot directly typethe HTTP request, but we'll see the response:

`curl -i https://flaviocopes.com/axios/`

this is what we'll get in return:

```
HTTP/1.1 200 OK
Cache-Control: public, max-age=0, must-revalidate
Content-Type: text/html; charset=UTF-8
Date: Sun, 29 Jul 2018 14:20:45 GMT
Etag: "de3153d6eacef2299964de09db154b32-ssl"
Strict-Transport-Security: max-age=31536000
Age: 152
Content-Length: 9797
Connection: keep-alive
Server: Netlify

<!DOCTYPE html>
<html prefix="og: http://ogp.me/ns#" lang="en">
<head>
<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<title>HTTP requests using Axios</title>
....
```

I cut the response, but you can see that the HTML of the page is being returned now.

### Other resources

An HTTP server will not just transfer HTML files, but typically it will also serve other files: CSS,JS, SVG, PNG, JPG, lots of different file types.

This depends on the configuration.

HTTP is perfectly capable of transferring those files as well, and the client will know about thefile type, thus interpret them in the right way.

This is how the web works: when an HTML page is retrieved by the browser, it's interpretedand any other resource it needs to display property (CSS, JavaScript, images..) is retrievedthrough additional HTTP requests to the same server.

## <a name="http-requests"></a> How HTTP Requests work

**What happens when you type an URL in the browser, from start to finish**

- [The HTTP protocol](http-protocol)
- [I analyze URL requests only](url-requests)
- [Things relate to macOS / Linux](things-relate)
- [DNS Lookup phase](dns-lookup)
  - [gethostbyname](gethostbyname)
- [TCP request handshaking](tcp-request-handshaking)
- [Sending the request](sending-the-request)
  - [The request line](request-line)
  - [The request header](request-header)
  - [The request body](request-body)
- [The response](the-response)
- [Parse the HTML](parse-html)

> This article describes how browsers perform page requests using the HTTP/1.1 protocol

If you ever did an interview, you might have been asked: "what happens when you typesomething into the Google search box and press enter".

It's one of the most popular questions you get asked. People just want to see if you canexplain some rather basic concepts and if you have any clue how the internet actually works.

In this post, I'll analyze what happens when you type an URL in the address bar of your browser and press enter.

It's a very interesting topic to dissect in a blog post, as it touches many technologies I can diveinto in separate posts.

This is tech that is very rarely changed, and powers one the most complex and wideecosystems ever built by humans.

### <a name="http-protocol"></a> The HTTP protocol

First, I mention HTTPS in particular because things are different from an HTTPS connection.

### <a name="url-requests"></a> I analyze URL requests only

Modern browsers have the capability of knowing if the thing you wrote in the address bar is anactual URL or a search term, and they will use the default search engine if it's not a valid URL.

I assume you type an actual URL.

When you enter the URL and press enter, the browser first builds the full URL.

If you just entered a domain, like `flaviocopes.com`, the browser by default will prepend `HTTP://` to it, defaulting to the HTTP protocol.

### <a name="things-relate"></a> Things relate to macOS / Linux

Just FYI. Windows might do some things slightly differently.

### <a name="dns-lookup"></a> DNS Lookup phase

The browser starts the DNS lookup to get the server IP address.

The domain name is a handy shortcut for us humans, but the internet is organized in such away that computers can look up the exact location of a server through its IP address, which isa set of numbers like `222.324.3.1` (IPv4).

First, it checks the DNS local cache, to see if the domain has already been resolved recently.

Chrome has a handy DNS cache visualizer you can see at chrome://net-internals/#dns

If nothing is found there, the browser uses the DNS resolver, using the gethostbyname POSIXsystem call to retrieve the host information.

#### <a name="gethostbyname"></a> gethostbyname

`gethostbyname` first looks in the local hosts file, which on macOS or Linux is located in `/etc/hosts`, to see if the system provides the information locally.

If this does not give any information about the domain, the system makes a request to theDNS server.

The address of the DNS server is stored in the system preferences.

Those are 2 popular DNS servers:

- `8.8.8.8`: the Google public DNS server

- `1.1.1.1`: the CloudFlare DNS server

Most people use the DNS server provided by their internet provider.

The browser performs the DNS request using the UDP protocol.

TCP and UDP are two of the foundational protocols of computer networking. They sit at thesame conceptual level, but TCP is connection-oriented, while UDP is a connectionlessprotocol, more lightweight, used to send messages with little overhead.

>How the UDP request is performed is not in the scope of this tutorial

The DNS server might have the domain IP in the cache. It not, it will ask the root DNS server.That's a system (composed of 13 actual servers, distributed across the planet) that drives theentire internet.

The DNS server does not know the address of each and every domain name on the planet.

What it knows is where the top-level DNS resolvers are.

A top-level domain is the domain extension: `.com`, `.it`, `.pizza` and so on.

Once the root DNS server receives the request, it forwards the request to that top-leveldomain (TLD) DNS server.

Say you are looking for `flaviocopes.com`. The root domain DNS server returns the IP of the `.com` TLD server.

Now our DNS resolver will cache the IP of that TLD server, so it does not have to ask the rootDNS server again for it.

The TLD DNS server will have the IP addresses of the authoritative Name Servers for thedomain we are looking for.

>How? When you buy a domain, the domain registrar sends the appropriate TDL thename servers. When you update the name servers (for example, when you change thehosting provider), this information will be automatically updated by your domain registrar.

Those are the DNS servers of the hosting provider. They are usually more than 1, to serve asbackup.

For example:

- `ns1.dreamhost.com`
- `ns2.dreamhost.com`
- `ns3.dreamhost.com`

The DNS resolver starts with the first, and tries to ask the IP of the domain (with thesubdomain, too) you are looking for.

That is the ultimate source of truth for the IP address.

Now that we have the IP address, we can go on in our journey.


### <a name="tcp-request-handshaking"></a> TCP request handshaking

With the server IP address available, now the browser can initiate a TCP connection to that.

A TCP connection requires a bit of handshaking before it can be fully initialized and you canstart sending data.

Once the connection is established, we can send the request.

### <a name="sending-the-request"></a> Sending the request

The request is a plain text document structured in a precise way determined by thecommunication protocol.

It's composed of 3 parts:

- the request line
- the request header
- the request body

#### <a name="request-line"></a> The request line

The request line sets, on a single line:

- the HTTP method
- the resource location
- the protocol version

Example:

`GET / HTTP/1.1`

#### <a name="request-header"></a> The request header

The request header is a set of `field: value` pairs that set certain values.

There are 2 mandatory fields, one of which is `Host`, and the other is `Connection`, while all theother fields are optional:

```
Host: flaviocopes.com
Connection: close
```

`Host` indicates the domain name which we want to target, while `Connection` is always set to `close` unless the connection must be kept open.

Some of the most used header fields are:

- `Origin`
- `Accept`
- `Accept-Encoding`
- `Cookie`
- `Cache-Control`
- `Dnt`

but many more exist.

The header part is terminated by a blank line.

#### <a name="request-body"></a> The request body

The request body is optional, not used in GET requests but very much used in POST requestsand sometimes in other verbs too, and it can contain data in JSON format.

Since we're now analyzing a GET request, the body is blank and we'll not look more into it.

### <a name="the-response"></a> The response

Once the request is sent, the server processes it and sends back a response.

The response starts with the status code and the status message. If the request is successfuland returns a 200, it will start with:

`200 OK`

The request might return a different status code and message, like one of these:

```
404 Not Found
403 Forbidden
301 Moved Permanently
500 Internal Server Error
304 Not Modified
401 Unauthorized
```

The response then contains a list of HTTP headers and the response body (which, since we'remaking the request in the browser, is going to be HTML)

### <a name="parse-html"></a> Parse the HTML

The browser now has received the HTML and starts to parse it, and will repeat the exact sameprocess we did not for all the resources required by the page:

- CSS files
- images
- the favicon
- JavaScript files
- ...

How browsers render the page then is out of the scope, but it's important to understand thatthe process I described is not just for the HTML pages, but for any item that's served over HTTP.

### <a name="http-server"></a> Build an HTTP server

**How to build an HTTP server with Node.js**

Here is the HTTP web server we used as the Node Hello World application in the [Node.js introduction](https://flaviocopes.com/nodejs/)

```
const http = require('http')

const port = 3000

const server = http.createServer((req, res) => {  
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/plain')
  res.end('Hello World\n')
})

server.listen(port, () => {
  console.log(`Server running at http://${hostname}:${port}/`)
})
```

Let's analyze it briefly. We include the [`http` module](https://nodejs.org/api/http.html).

We use the module to create an HTTP server.

The server is set to listen on the specified port, `3000`. When the server is ready, the `listen` callback function is called.

The callback function we pass is the one that's going to be executed upon every request thatcomes in. Whenever a new request is received, the [`request` event](https://nodejs.org/api/http.html#http_event_request) is called, providing twoobjects: a request (an [http.IncomingMessage](https://nodejs.org/api/http.html#http_class_http_incomingmessage) object) and a response (an [http.ServerResponseobject](https://nodejs.org/api/http.html#http_class_http_serverresponse)).

`request` provides the request details. Through it, we access the request headers and request data.

`response` is used to populate the data we're going to return to the client.

In this case with

`res.statusCode = 200`

we set the statusCode property to 200, to indicate a successful response.

We also set the Content-Type header:

`res.setHeader('Content-Type', 'text/plain')`

and we end close the response, adding the content as an argument to `end()`:

`res.end('Hello World\n')`

## <a name="http-requests"></a> Making HTTP requests

**How to perform HTTP requests with Node.js using GET, POST, PUT and DELETE**

>I use the term HTTP, but HTTPS is what should be used everywhere, therefore theseexamples use HTTPS instead of HTTP.

### Perform a GET Request

```
const https = require('https')

const options = {
  hostname: 'flaviocopes.com',  
  port: 443,  
  path: '/todos',  
  method: 'GET'
}

const req = https.request(options, (res) => {
  console.log(`statusCode: ${res.statusCode}`)  
  
  res.on('data', (d) => {    
    process.stdout.write(d)  
  })
})

req.on('error', (error) => {
  console.error(error)
})

req.end()
```

### Perform a POST Request

```
const https = require('https')

const data = JSON.stringify({  
  todo: 'Buy the milk'
})

const options = {  
  hostname: 'flaviocopes.com',  
  port: 443,  
  path: '/todos',  
  method: 'POST',  
  headers: {
    'Content-Type': 'application/json',
    'Content-Length': data.length  
  }
}

const req = https.request(options, (res) => {
  console.log(`statusCode: ${res.statusCode}`)  
  
  res.on('data', (d) => {    
    process.stdout.write(d)  
  })
})

req.on('error', (error) => {
  console.error(error)
})

req.write(data)
req.end()
```

### PUT and DELETE

PUT and DELETE requests use the same POST request format, and just change the `options.method` value.

## <a name="axios"></a> Axios

**Axios is a very convenient JavaScript library to perform HTTP requests inNode.js**

- [Introduction](introduction-axios)
- [Installation](installation-axios)
- [The Axios API](axios-api)
- [GET requests](axios-get)
- [Add parameters to GET requests](axios-parameters)
- [POST Requests](axios-post)

### <a name="introduction-axios"></a> Introduction

Axios is a very popular JavaScript library you can use to perform HTTP requests, that works inboth Browser and [Node.js](https://flaviocopes.com/nodejs/) platforms.

![Axios](/images/Capitulo-7/axios.png)

It supports all modern browsers, including support for IE8 and higher.

It is promise-based, and this lets us write async/await code to perform [XHR](https://flaviocopes.com/xhr/) requests very easily.

Using Axios has quite a few advantages over the native [Fetch API](https://flaviocopes.com/fetch-api/):

- supports older browsers (Fetch needs a polyfill)
- has a way to abort a request
- has a way to set a response timeout
- has built-in CSRF protection
- supports upload progress
- performs automatic JSON data transformation
- works in Node.js

### <a name="installation-axios"></a> Installation

Axios can be installed using [npm](https://flaviocopes.com/npm/):

`npm install axios`

or [yarn](https://flaviocopes.com/yarn/):

`yarn add axios`

or simply include it in your page using unpkg.com:

`<scriptsrc="https://unpkg.com/axios/dist/axios.min.js"></script>`

### <a name="axios-api"></a> The Axios API

You can start an HTTP request from the `axios` object:

```
axios({  
  url: 'https://dog.ceo/api/breeds/list/all',  
  method: 'get',  
  data: {    
    foo: 'bar'  
  }
})
```

but for convenience, you will generally use

- `axios.get()`
- `axios.post()`

(like in jQuery you would use `$.get()` and `$.post()` instead of `$.ajax()`)

Axios offers methods for all the HTTP verbs, which are less popular but still used:

- `axios.delete()`
- `axios.put()`
- `axios.patch()`
- `axios.options()`

and a method to get the HTTP headers of a request, discarding the body:

- `axios.head()`

### <a name="axios-get"></a> GET requests

One convenient way to use Axios is to use the modern (ES2017) async/await syntax.

This Node.js example queries the [Dog API](https://dog.ceo/) to retrieve a list of all the dogs breeds, using `axios.get()`, and it counts them:

```
const axios = require('axios')

const getBreeds = async () => {
  try {
    returnawait axios.get('https://dog.ceo/api/breeds/list/all')  
  } catch (error) {
    console.error(error)  
  }
}

const countBreeds = async () => {
  const breeds = await getBreeds()
  
  if (breeds.data.message) {
    console.log(`Got ${Object.entries(breeds.data.message).length} breeds`)  
  }
}

countBreeds()
```

If you don't want to use async/await you can use the [Promises](https://flaviocopes.com/javascript-promises/) syntax:

```
const axios = require('axios')

const getBreeds = () => {
  try {
    return axios.get('https://dog.ceo/api/breeds/list/all')  
  } catch (error) {
    console.error(error)  
  }
}

const countBreeds = async () => {
  const breeds = getBreeds()
  .then(response => {
    if (response.data.message) {
      console.log(
        `Got ${Object.entries(response.data.message).length} breeds`        
      )      
    }    
  })    
  .catch(error => {
    console.log(error)    
  })
}

countBreeds()

```

### <a name="axios-parameters"></a> Add parameters to GET requests

A GET response can contain parameters in the URL, like this: `https://site.com/?foo=bar`. 

With Axios you can perform this by simply using that URL:

`axios.get('https://site.com/?foo=bar')`

or you can use a `params` property in the options:

```
axios.get('https://site.com/', {  
  params: {    
    foo: 'bar'  
  }
})
```

### <a name="axios-post"></a> POST Requests

Performing a POST request is just like doing a GET request, but instead of `axios.get`, youuse `axios.post`:

`axios.post('https://site.com/')`

An object containing the POST parameters is the second argument:

```
axios.post('https://site.com/', {  
  foo: 'bar'
})
```

## <a name="websockets"></a> Websockets

**WebSockets are an alternative to HTTP communication in Web Applications.They offer a long lived, bidirectional communication channel between clientand server.**

WebSockets are an alternative to HTTP communication in Web Applications.

They offer a long lived, bidirectional communication channel between client and server.

Once established, the channel is kept open, offering a very fast connection with low latencyand overhead.

### Browser support for WebSockets

WebSockets are supported by all modern browsers.

![Browser Support](/images/Capitulo-7/browser-support.png)

### How WebSockets differ from HTTP

HTTP is a very different protocol, and also a different way of communicate.

HTTP is a request/response protocol: the server returns some data when the client requests it.

With WebSockets:

- the server can send a message to the client without the client explicitly requesting something
- the client and the server can talk to each other simultaneously
- very little data overhead needs to be exchanged to send messages. This means a low latency communication.

WebSockets are great for real-time and long-lived communications.

HTTP is great for occasional data exchange and interactions initiated by the client.

HTTP is much simpler to implement, while WebSockets require a bit more overhead.

### Secured WebSockets

Always use the secure, encrypted protocol for WebSockets, `wss://`.

`ws://` refers to the unsafe WebSockets version (the `http://` of WebSockets), and should beavoided for obvious reasons.

### Create a new WebSockets connection

```
const url = 'wss://myserver.com/something'
const connection = new WebSocket(url)
```

`connection` is a [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) object.

When the connection is successfully established, the `open` event is fired.

Listen for it by assigning a callback function to the `onopen` property of the `connection` object:

```
connection.onopen = () => {
  //...  
}
```
If there's any error, the `onerror` function callback is fired:

```
connection.onerror = error => {
  console.log(`WebSocket error: ${error}`)
}
```

### Sending data to the server using WebSockets

Once the connection is open, you can send data to the server.

You can do so conveniently inside the `onopen` callback function:

```
connection.onopen = () => {  
  connection.send('hey')
}
```

### Receiving data from the server usingWebSockets

Listen with a callback function on `onmessage`, which is called when the message event is received:

```
connection.onmessage = e => {
  console.log(e.data)
}
```

### Implement a WebSockets server in Node.js

[ws](https://github.com/websockets/ws) is a popular WebSockets library for [Node.js](https://flaviocopes.com/nodejs).

We'll use it to build a WebSockets server. It can also be used to implement a client, and useWebSockets to communicate between two backend services.

Easily install it using: 

```
yarn init
yarn add ws
```

The code you need to write is very little:

```
const WebSocket = require('ws')

const wss = new WebSocket.Server({ port: 8080 })

wss.on('connection', ws => {  
  ws.on('message', message => {
    console.log(`Received message => ${message}`)  
  })    
  ws.send('ho!')
})
```

This code creates a new server on port 8080 (the default port for WebSockets), and adds acallback function when a connection is established, sending `ho!` to the client, and logging themessages it receives.

### See a live example on Glitch

Here is a live example of a WebSockets server: [https://glitch.com/edit/#!/flavio-websockets-server-example](https://glitch.com/edit/#!/flavio-websockets-server-example)

Here is a WebSockets client that interacts with the server: [https://glitch.com/edit/#!/flavio-websockets-client-example](https://glitch.com/edit/#!/flavio-websockets-client-example])

## <a name="secure-connections"></a> HTTPS, secure connections

**The HTTPS protocol is an extension of HTTP, the Hyper Text TransferProtocol, that provide secure communication**

HTTP in insecure by design.

When you open your browser and ask a web server to send you a webpage, your dataperforms 2 trips: 1 from the browser to the web server, and 1 from the web server to the browser.

Then, depending on the content of the web page, you might have more connections requiredto get the CSS files, the JavaScript files, images, and so on.

During any of those connections, any network your data is going to cross can be **inspected** and **manipulated**.

The consequences can be serious: you might have all your network activity monitored andlogged, by a 3rd part you are not even aware it exist, some networks [might inject ads](https://justinsomnia.org/2012/04/hotel-wifi-javascript-injection/), and youmight be subject to a man-in-the-middle attack, a security threat where the attacker canmanipulate your data and even impersonate your computer over the network. It's very easy forsomeone to just listen to HTTP packets being transmitted over a public and unencrypted Wi-Fi network.

HTTPS aims to solve the problem at the root: the entire communication between your browserand the web server is encrypted.

Privacy and security are a major concern in today's internet. A few years ago, you could getaway with just using an encrypted connection in login-protected pages, or during an e-commerce checkout. Also because of SSL certificates pricing and complications, mostwebsites just used HTTP.

Today HTTPS is a requirement on any site. More than 50% of the whole Web uses it now.Google Chrome recently started marking HTTP sites as insecure, just to give you a validreason to have HTTPS mandatory (and forced) on all your websites.

When using HTTP the default server port is 80, and on HTTPS it's 443. It does not need to beexplicitly added if the server uses the default port, of course.

HTTPS is also sometimes called HTTP over SSL, or HTTP over TLS.

The difference between the two is simple: TLS is the successor of SSL.

When using HTTPS, the only thing that is not encrypted is the web server domain, and the server port.

Every other information, including the resource path, headers, cookies and query parameters are all encrypted.

I won't go in the details of analyzing how the TLS protocol works under the hoods, but youmight think it's adding a good amount of **overhead**, and you would be right.

Any computation that's added to the processing of network resources causes overhead bothon the client, the server, and to the transmitted packets size.

However HTTPS enables the use of the newest protocol **HTTP/2**, which has a hugeadvantage over HTTP/1.1: it way faster.

Why? There are many reasons, one is header compression, one is resource multiplexing. Oneis server push: the server can push more resources when one resource is requested. So, if thebrowser requests a page, it will also receive all the resources needed (images, CSS, JS).

Details aside, HTTP/2 is a huge improvement over HTTP/1.1 **and it requires HTTPS**. Thismeans that HTTPS, despite having the encryption overhead, happens to be way faster thanHTTP, if things are properly configured with a modern setup.


