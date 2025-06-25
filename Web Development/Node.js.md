Node.js is an engine which is used to run javascript code on your machine. What happened is that javascript was a language developed to run on browsers. So, all the browsers had engines to run javascript code, like:
- Chrome -> V8 engine
- Firefox -> Spider monkey
- Safari -> Apple javascript engine

![[Pasted image 20241207214645.png]]

Ryan Dahl, what he did was that he made a V8 based javascript compiler using C++ that can be installed on your pc and named it Node.
**Node.js  is a runtime environment for javascript.**
## Difference between Node.js and V8
![[Pasted image 20241207214816.png]]
The javascript syntax is pretty much the same for the most part, but some inbuilt objects and functions are different. Like for web engines, you have objects like document, HTML, DOM, etc. but for Node.js you have objects like module and functions like require.
## NPM: Node Package Manager
Another key program we need to discuss is npm. It helps you manage your own Node package and also makes it easy for you to install and configure node packages created by other developers.
We start a new node.js project by running the `npm init` command which will initialize the node folder for you with `package.json` which has all your configurations along with starting point, package name, description, etc.
# Modules
Node's most prominent feature is it's modules, let's say we want to make a module of math, we can do it in this way:

```javascript
// math.js
const pi = 3.14;
const e = 2.8;
module.exports = {
	"add": (a, b) => a+b,
	"sub": (a, b) => a-b,
	"mul": (a, b) => a*b,
	"div": (a, b) => a/b,
	pi, e,
}
```

```javascript
// app.js
const math = require('./math.js');
console.log(math.add(2, 4)); // 6
console.log(math.pi); // 3.14
```

OR, there is another way to handle exports, we can also do it as:

```javascript
// math.js
exports.add = (a, b) => a+b;
exports.sub = (a, b) => a-b;
exports.mul = (a, b) => a*b;
exports.div = (a, b) => a/b;
exports.pi = 3.14;
```

# File System
`const fs = require('fs');`

| Function                                                                | Description                                                                                                                            |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| `fs.writeFile(string:path, string:data, err => {});`                    | Write content to a given file at path, if not present, it will create the file. We also provide a callback function in case of error   |
| `fs.writeFileSync(string:path, string:data);`                           | Synchronously write the file                                                                                                           |
| `fs.readFile(string:path, string:encoding/"utf-8", (err, data) => {});` | Reads a file asynchronously and provides the data in the callback function                                                             |
| `fs.readFileSync(string:path, string:endcoding/"utf-8");`               | Reads the file synchronously and returns the result                                                                                    |
| `fs.appendFile(string:path, string:data, err => {});`                   | Appends content to a given file at path, if not present, it will create the file. We also provide a callback function in case of error |
| `fs.appendFileSync(string:path, string:data);`                          | Synchronously appends to the file                                                                                                      |
| `fs.unlinkSync(string:path);`                                           | Deletes the file at given path                                                                                                         |
| `fs.statSync(string:path);`                                             | Returns an object containing all the stats of the file like size, create time, modification time, etc.                                 |
| `fs.mkdir(string:name, object:options);`                                | Makes the directory                                                                                                                    |
# Node Architecture
When client makes a request to our Node.js server, it gets pushed to the *Event Queue*, which then gets processed in the *Event Loop*.
The provided request can be
- Blocking operation: Synchronous
- Non-blocking operations: Asynchronous 

Non-blocking operations are directly ran and the response is sent to the user via the callback.
But for blocking operations, we assign that operation to a thread pool which will run that blocking operation on a separate thread which then delivers the response to the user.

>The default thread pool size is 4 because the standard computer has 4 cores. However, we can have more threads by getting a server with more cores.
>We can check the number of cores by using os module `const os = require('os')` and then using the cpus function like `os.cpus().length`

```javascript
const fs = require('fs');

console.log('1.1');
console.log(fs.readFileSync('text.txt','utf-8'));
console.log('1.2');

console.log('2.1');
fs.readFile('text.txt','utf-8', (err, data) => {console.log(data)});
console.log('2.2');
```
Output:
```
1.1
Hello World
1.2
2.1
2.2
Hello World
```

# HTTP Module

```javascript
const http = require('http');
const fs = require('fs');
const server = http.createServer((req, res) => {
	const log = `${Date.now()}: ${req.url} Req Received`;
	console.log(log);
	fs.appendFile('./log.txt', log);
	res.end("Hello from server");
});
server.listen(8000, () => console.log("Server Started"));
```

Now I can open `http://localhost:8000` on my browser after running the above file and getting the Server Started message. Every time I refresh the page, it'll add that to the log.txt file and also log it on console and my website will show Hello from server.
You can close the server by `Ctrl+C` in the terminal on which the file is running.
## URL (Uniform Resource Locator)
`https://meetshah.dev/home`
- `https://` Defines the protocol of the data transfer
- `meetshah.dev/` Human friendly name for the IP address of a server
- `home` Path to the url in the server
Interesting fact: You can even enter the IP address of the server you want to reach in the address bar of your browser. Try: 142.250.192.174
### Paths
Paths can be of many types:
- Simple paths: `meetshah.dev/home`
- Nested paths: `meetshah.dev/projects/1`
- Query parameters: `meetshah.dev/about?userName=meet+shah&userId=1`

We can simplify decoding url paths using the url package in npm. So we can run `npm i url` to download the url package.
```javascript
const url = require("url");
// ...
	const myUrl = url.parse(req.url, true);
// ...
```
The true we passed as a second argument indicates url object to also parse the query string and break it into an object.
```javascript
// https://meetshah.dev/about?userName=meet&userId=1&search=dog
myUrl = {
	protocol: null,
	host: null,
	port: null,
	search: "?userName=meet&userId=1&search=dog",
	query: {
		userName: "meet",
		userId: "1",
		search: "dog",
	},
	pathname: "/about",
	path: "/about?userName=meet&userId=1&search=dog",
}
// Protocol, host and port are null because we are using localhost
```

## HTTP Methods
HTTP request method can be accessed by `req.method`

| Method | Description                                |
| ------ | ------------------------------------------ |
| GET    | When you want to get data from server      |
| POST   | When you want to add data to a server      |
| PUT    | When you want to upload a file to a server |
| PATCH  | When you want to change data on a server   |
| DELETE | When you want to delete data from a server |
# Versioning
Example -> `4.18.2`
- `4`: Major release / Breaking update
- `18`: Bug fixes, addition/subtraction of features
- `2`: Minor fixes and changes (optional)

When we install npm packages, in the package.json file, the version of the dependency is shown as `^4.18.2` which suggests that when reinstalling the packages, it will allow bug fixes and minor fixes but not change the major version. 
If the version was to be written as `4.18.2`, then it would lock on that version and not even perform minor updates. 
`~4.18.2` updates only minor fixes and doesn't change the bug fixes and the major version.
We can even provide a range as `4.18.0 - 4.19.0`
Or you can even do `latest`.
Or we can also fix the first two like `4.18.x`
