<h1 align="center"> Outros </h1>

- [Streams](#streams)
- [Working with MySQL](#working-mysql)
- [Difference between development and production](#difference-development-production)

## <a name="streams"></a> Streams

**Learn what streams are for, why are they so important, and how to use them.**

- [What are streams](#what-are-streams)
- [Why streams](#why-streams)
- [An example of a stream](#example-of-stream)
- [pipe()](#pipe)
- [Streams-powered Node APIs](#powered-node-apis)
- [Different types of streams](#different-types)
- [How to create a readable stream](#readable-stream)
- [How to create a writable stream](#writable-stream)
- [How to get data from a readable stream](#get-data-from-readable-stream)
- [How to send data to a writable stream](#send-data-to-writable-stream)
- [Signaling a writable stream that you ended writing](#signaling-writable-stream)
- [Conclusion](#streams-conclusion)

### <a name="what-are-streams"></a> What are streams

Streams are one of the fundamental concepts that power Node.js applications.

They are a way to handle reading/writing files, network communications, or any kind of end-to-end information exchange in an efficient way.

Streams are not a concept unique to Node.js. They were introduced in the Unix operatingsystem decades ago, and programs can interact with each other passing streams through thepipe operator (`|`).

For example, in the traditional way, when you tell the program to read a file, the file is read intomemory, from start to finish, and then you process it.

Using streams you read it piece by piece, processing its content without keeping it all in memory.

The Node.js [`stream` module](https://nodejs.org/api/stream.html) provides the foundation upon which all streaming APIs are build.

### <a name="why-streams"></a> Why streams

Streams basically provide two major advantages using other data handling methods:

- **Memory efficiency**: you don't need to load large amounts of data in memory before youStreams178
are able to process it

- **Time efficiency**: it takes way less time to start processing data as soon as you have it,rather than waiting till the whole data payload is available to start

### <a name="example-of-stream"></a> An example of a stream

A typical example is the one of reading files from a disk.

Using the Node `fs` module you can read a file, and serve it over HTTP when a newconnection is established to your http server:

```
const http = require('http')
const fs = require('fs')

const server = http.createServer(function (req, res) {  
  fs.readFile(__dirname + '/data.txt', (err, data) => {    
    res.end(data)  
  })
})
server.listen(3000)
```  
    
`readFile()` reads the full contents of the file, and invokes the callback function when it's done.

`res.end(data)` in the callback will return the file contents to the HTTP client.

If the file is big, the operation will take quite a bit of time. Here is the same thing written using streams:

```
const http = require('http')
const fs = require('fs')

const server = http.createServer((req, res) => {
  const stream = fs.createReadStream(__dirname + '/data.txt')  
  stream.pipe(res)
})
server.listen(3000)
```

Instead of waiting until the file is fully read, we start streaming it to the HTTP client as soon aswe have a chunk of data ready to be sent.

### <a name="pipe"></a> pipe()

The above example uses the line `stream.pipe(res)`: the `pipe()` method is called on the file stream.

What does this code do? It takes the source, and pipes it into a destination.

You call it on the source stream, so in this case, the file stream is piped to the HTTP response.

The return value of the `pipe()` method is the destination stream, which is a very convenientthing that lets us chain multiple `pipe()` calls, like this:

`src.pipe(dest1).pipe(dest2)`

This construct is the same as doing

```
src.pipe(dest1)
dest1.pipe(dest2)
```

### <a name="powered-node-apis"></a> Streams-powered Node APIs

Due to their advantages, many Node.js core modules provide native stream handling capabilities, most notably:

- `process.stdin` returns a stream connected to stdin
- `process.stdout` returns a stream connected to stdout
- `process.stderr` returns a stream connected to stderr
- `fs.createReadStream()` creates a readable stream to a file
- `fs.createWriteStream()` creates a writable stream to a file
- `net.connect()` initiates a stream-based connection
- `http.request()` returns an instance of the http. ClientRequest class, which is a writablestream
- `zlib.createGzip()` compress data using gzip (a compression algorithm) into a stream
- `zlib.createGunzip()` decompress a gzip stream.
- `zlib.createDeflate()` compress data using deflate (a compression algorithm) into a stream
- `zlib.createInflate()` decompress a deflate stream

### <a name="different-types"></a> Different types of streams

There are four classes of streams:

- Readable: a stream you can pipe from, but not pipe into (you can receive data, but notsend data to it). When you push data into a readable stream, it is buffered, until aconsumer starts to read the data.
- Writable: a stream you can pipe into, but not pipe from (you can send data, but notStreams180
receive from it)
- Duplex: a stream you can both pipe into and pipe from, basically a combination of aReadable and Writable stream
- Transform: a Transform stream is similar to a Duplex, but the output is a transform of its input

### <a name="readable-stream"></a> How to create a readable stream

We get the Readable stream from the [`stream` module](https://nodejs.org/api/stream.html), and we initialize it

```
const Stream = require('stream')
const readableStream = new Stream.Readable()
```

Now that the stream is initialized, we can send data to it:

```
readableStream.push('hi!')
readableStream.push('ho!')
```

### <a name="writable-stream"></a> How to create a writable stream

To create a writable stream we extend the base `Writable` object, and we implement its_write() method.

First create a stream object:

```
const Stream = require('stream')
const writableStream = new Stream.Writable()
```

then implement `_write`:

```
writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString())    
  next()
}
```

You can now pipe a readable stream in:

`process.stdin.pipe(writableStream)`

### <a name="get-data-from-readable-stream"></a> How to get data from a readable stream

How do we read data from a readable stream? Using a writable stream:

```
const Stream = require('stream')

const readableStream = new Stream.Readable()
const writableStream = new Stream.Writable()

writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString())    
  next()
}

readableStream.pipe(writableStream)

readableStream.push('hi!')
readableStream.push('ho!')
```

You can also consume a readable stream directly, using the `readable` event:

```
readableStream.on('readable', () => {
  console.log(readableStream.read())
}
```

### <a name="send-data-to-writable-stream"></a> How to send data to a writable stream

Using the stream `write()` method:

`writableStream.write('hey!\n')`

### <a name="signaling-writable-stream"></a> Signaling a writable stream that you ended writing

Use the` end()` method:

```
const Stream = require('stream')

const readableStream = new Stream.Readable()
const writableStream = new Stream.Writable()

writableStream._write = (chunk, encoding, next) => {
  console.log(chunk.toString())    
  next()
}

readableStream.pipe(writableStream)
```

### <a name="streams-conclusion"></a> Conclusion

This is an introduction to streams. There are much more complicated aspects to analyze, and Ihope to cover them soon.

## <a name="working-mysql"></a> Working with MySQL

**MySQL is one of the most popular relational databases in the world. Find outhow to make it work with Node.js**

MySQL is one of the most popular relational databases in the world.

The Node ecosystem of course has several different packages that allow you to interface withMySQL, store data, retrieve data, and so on.

We'll use `mysqljs/mysql`, a package that has over 12.000 GitHub stars and has been around for years.

### Installing the Node mysql package

You install it using

`npm install mysql`

### Initializing the connection to the database

You first include the package:

`const mysql = require('mysql')`

and you create a connection:

```
const options = {  
  user: 'the_mysql_user_name',  
  password: 'the_mysql_user_password',  
  database: 'the_mysql_database_name'
}
const connection = mysql.createConnection(options)
```

You initiate a new connection by calling:

```
connection.connect(err => {
  if (err) {
    console.error('An error occurred while connecting to the DB')
    throw err  
  }
})
```

### The connection options

In the above example, the options object contained 3 options:

```
const options = {  
  user: 'the_mysql_user_name',  
  password: 'the_mysql_user_password',  
  database: 'the_mysql_database_name'
}
```

There are many more you can use, including:

- `host`, the database hostname, defaults to `localhost`
- `port`, the MySQL server port number, defaults to 3306
- `socketPath`, used to specify a unix socket instead of host and port
- `debug`, by default disabled, can be used for debugging
- `trace`, by default enabled, prints stack traces when errors occur
- `ssl`, used to setup an SSL connection to the server (out of the scope of this tutorial)

### Perform a SELECT query

Now you are ready to perform an SQL query on the database. The query once executed willinvoke a callback function which contains an eventual error, the results and the fields.

```
connection.query('SELECT * FROM todos', (error, todos, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')
    throw error  
  }
  console.log(todos)
})
```

You can pass in values which will be automatically escaped:

```
const id = 223
connection.query('SELECT * FROM todos WHERE id = ?', [id], (error, todos, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')
    throw error  
  }
  console.log(todos)
})
```

To pass multiple values, just put more elements in the array you pass as the secondparameter:

```
const id = 223
const author = 'Flavio'
connection.query('SELECT * FROM todos WHERE id = ? AND author = ?', [id, author], (error, todos, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')throw error  
  }
  console.log(todos)
})
```

### Perform an INSERT query

You can pass an object

```
const todo = {  
  thing: 'Buy the milk'  
  author: 'Flavio'
}
connection.query('INSERT INTO todos SET ?', todo, (error, results, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')throw error  
  }
})
```

If the table has a primary key with `auto_increment`, the value of that will be returned in t`heresults.insertId` value:

```
const todo = {  
  thing: 'Buy the milk'  
  author: 'Flavio'
}
connection.query('INSERT INTO todos SET ?', todo, (error, results, fields) => {
  if (error) {
    console.error('An error occurred while executing the query')
    throw error  
  }}
  const id = results.resultId
  console.log(id)
)
```

### Close the connection

When you need to terminate the connection to the database you can call the `end()` method:

`connection.end()`

This makes sure any pending query gets sent, and the connection is gracefully terminated.

## <a name="difference-development-production"></a> Difference between development and production

**Learn how to set up different configurations for production and developmentenvironments**

You can have different configurations for production and development environments.

Node assumes it's always running in a development environment. You can signal Node.js thatyou are running in production by setting the `NODE_ENV=production` environment variable.

This is usually done by executing the command

`export NODE_ENV=production`

in the shell, but it's better to put it in your shell configuration file (e.g. `.bash_profile` with theBash shell) because otherwise the setting does not persist in case of a system restart.

You can also apply the environment variable by prepending it to your application initialization command:

`NODE_ENV=production node app.js`

This environment variable is a convention that is widely used in external libraries as well.

Setting the environment to `production` generally ensures that

- logging is kept to a minimum, essential level
- more caching levels take place to optimize performance

For example Pug, the templating library used by Express, compiles in debug mode if `NODE_ENV` is not set to `production`. Express views are compiled in every request indevelopment mode, while in production they are cached. There are many more examples.

Express provides configuration hooks specific to the environment, which are automaticallycalled based on the NODE_ENV variable value:

```
app.configure('development', () => {
  //...
})
app.configure('production', () => {
  //...
})
app.configure('production', 'staging', () => {
  //...
})
```

For example you can use this to set different error handlers for different mode:

```
app.configure('development', () => {  
  app.use(express.errorHandler({ dumpExceptions: true, showStack: true }));
})
app.configure('production', () => {  
  app.use(express.errorHandler())
})
```

