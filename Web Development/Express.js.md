Making a web server using Node.js can be very frustrating when the server gets big and complex. So we use express.js, for example, here is the same server in both Node.js and Express.js:

```javascript
// Node.js
const http = require("http");
const url = require("url");

const server = http.createServer((req, res) => {
	const myUrl = url.parse(req.url, true);
	const method = req.method;
	switch(myUrl.pathname) {
		case "/":
			if(method=="GET") {
				// Do the GET code for Homepage
			} else if (method == "POST") {
				// Do the POST code for Homepage
			}
		case "/about":
			if(method=="GET") {
				// Do the GET code for About page
			} else if (method == "POST") {
				// Do the POST code for About page
			}
	}
})
server.listen(8000, () => console.log("Server Started"));
```

```javascript
// Express.js
const express = require("express");

const app = express();

app.get("/", (req, res) => {
	// Do the GET code for Homepage
})
app.get("/about", (req, res) => {
	// Do the GET code for About page
})
app.post("/", (req, res) => {
	// Do the POST code for Homepage
})
app.post("/about", (req, res) => {
	// Do the POST code for About page
})

app.listen(8000, () => console.log("Server Started"));
```
## Multi Method Handling
```javascript
app.all("/home", (req, res) => {
	// Code for all methods on home
})
// OR
app.route("/home")
	.get((req, res) => {})
	.put((req, res) => {})
	.patch((req, res) => {})
	.delete((req, res) => {})
```

In order to send a response, we can use:
- `res.send()`: Sends a text 
- `res.render()`: Sends a html file
- `res.json()`: Sends a json file
## Request and Response

| Request Method   | Description                                                                                  |
| ---------------- | -------------------------------------------------------------------------------------------- |
| `req.app`        | Instance of the app object                                                                   |
| `req.baseUrl`    | Base url on which route is mounted                                                           |
| `req.orignalUrl` | The full url                                                                                 |
| `req.body`       | Contains body usually in POST method, requires middleware like express.json() or body-parser |
| `req.cookies`    | Contains cookies, requires cookie-parser middleware                                          |
| `req.ip`         | Gives IP address of requesting server                                                        |
| `req.method`     | Gives type of method (get, post, put..)                                                      |
| `req.params`     | Gives route parameters                                                                       |
| `req.protocol`   | HTTP or HTTPS (you can also use `req.secure`)                                                |
| `req.get(field)` | Returns value of a request header                                                            |
| `req.is(type)`   | Checks the content-type of the request                                                       |

| Response Method                              | Description                                                                                                                   |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `res.app`                                    | Instance of the app object                                                                                                    |
| `res.status(code)`                           | Sets the status of the response                                                                                               |
| `res.send(body)`                             | Sends a response body which can be string or object                                                                           |
| `res.json(obj)`                              | Sends a json response                                                                                                         |
| `res.sendFile(path, [options], [callback])`  | Sends a file                                                                                                                  |
| `res.download(path, [filename], [callback])` | Prompts the user to download file                                                                                             |
| `res.render(view, [locals], [callback])`     | Renders an HTML file                                                                                                          |
| `res.redirect(status, url)`                  | Redirects to another url                                                                                                      |
| `res.set(field, value)`                      | Sets a response header                                                                                                        |
| `res.get(field)`                             | Returns value of response header                                                                                              |
| `res.cookie(name, value, [options])`         | Sets a cookie in response                                                                                                     |
| `res.clearCookie(name, [options])`           | Deletes a cookie                                                                                                              |
| `res.format(obj)`                            | Sends a response based on `Accept` header, the obj contains 3 content types: `text/plain`, `text/html` and `application/json` |
# REST APIs
*Representational State Transfer*
Restful APIs are APIs which follow a specific set of rules for their structure.
1. *Server-Client Architecture*: Client sends a request and server sends a response.
2. *Stateless Communication*: Every request from client contains all the information the server needs to process it and the server doesn't need to store any information of the client.
3. *Uniform Interface*: RESTful APIs follow HTTP methods.
4. *Resource-Oriented*: Everything is a resource with a specific URL.
5. *Stateless Caching*: Responses can be cached to improve performance.

**Types of Rendering:**
- *SSR:* Server side rendering is a type of rendering in which when the client sends a request, we send an HTML page as a response which is built on our server. This is used when we are sure that the client is a browser and can display html data.
- *CSR:* Client side rendering is a type of rendering in which when the client sends a request, we send a XML or JSON file which is then rendered into HTML on the client's machine. This is used when we are unsure if the client can render html or our html page is very computationally heavy to build and puts load on our server.
# Middlewares
Middlewares are functions which execute between the client and the server. The request sent by client is first processed in the middleware and then it decides wether to forward that request to the server or not and can even forward a modified request to the server.
![Middleware](attachments/middleware.png)
We can add a middleware using the `app.use(middleware)` function. It takes in a function as a middleware which has 3 parameters:
- `req`: The request
- `res`: The response
- `next`: The next middleware or the server

```javascript
// Middleware 1
app.use((req, res, next) => {
	console.log("Checking your username...");
	req.usernameCheck = true;
	next();
})
// Middleware 2 
app.use((req, res, next) => {
	return res.json({ msg: "You might be a threat!" })
})
```