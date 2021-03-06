# HTTP Request and Response Cycle
<h5>Introductory reference sheet prepared by Jeff George<br>
for future students at Dev Bootcamp</h5>

## TL;DR version

![HTTP Response-Request Cycle](https://github.com/webdevjeffus/reference_sheets/blob/master/imgs/http-request-response.png)
_Image from [https://www.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html]_

### The Client sends a _Request_ to the Server
* Requests start with a **request line**, consisting of a verb, a URL, and an HTTP version number. The verb tells the server what to do, the URL tells the server where to do it; ignore the version number, it's always 1.1.
```
GET http://www.devbootcamp.com HTTP/1.1
---  ------------------------  -------
verb            URL            version
```
* Requests have a header, to carry meta-data, and a body, to carry content data.
* A GET request has an empty body.
* The body of a POST request typically carries data from HTML forms submitted by the user, to be used by the server in some way.

### The Server sends a _Response_ back to the Client
* Responses begin with a status line, containing the HTTP version number, a status code and a description, which describes the result of the request.

Status Code | Meaning
---|---
200+ | Success.
300+ | Redirect.
400+ | Client error (usually a non-existent URL)
500+ | Server error (usually a bug in the server-side code)

* Responses also consiste of a header (meta-data) and a body (content).
* A successful GET request will receive a status-200 response, and its body will contain a "view", consisting of an HTML file rendered as a string.
* A successful POST request will receive a status-300 response, indicating that the server was able to process the data, along with a **redirect**, instructing the client to send a GET request to another URL to receive a new view.

#### The TL;DR Takeaway
* A successful **GET** request receives a status-200 response carrying a **rendered view**.
* A successful **POST** request receives a status-300 response carrying a **redirect**.

### Flow of the Response-Request Cycle
```
+----------------+                                     +----------------+
|                |               INTERNET              |                |
|                |                                     |                |
|                |   Request: GET http://www.dbc.com   |                |
|                |---------------------------------->> |                |
|                |                                     |                |
|                |          Response: 200 - OK         |                |
|                | <<==================================|                |
|                |          HTML for home page,        |                |
|                |         rendered as a string        |                |
|                |                                     |                |
|                |                                     |                |
|                |                                     |                |
|                |                                     |                |
|                |       Request: POST /users/new      |                |
|                |==================================>> |                |
|     CLIENT     |       Data from new user form       |     SERVER     |
|                |                                     |                |
|                |       Response: 300 - Redirect      |                |
|                | <<----------------------------------|                |
|                |             to /users/237           |                |
|                |                                     |                |
|                |                                     |                |
|                |                                     |                |
|                |                                     |                |
|                |       Request: GET /users/237       |                |
|                |---------------------------------->> |                |
|                |                                     |                |
|                |          Response: 200 - OK         |                |
|                | <<==================================|                |
|                |   HTML for user #237 profile page,  |                |
|                |         rendered as a string        |                |
|                |                                     |                |
+----------------+                                     +----------------+
```

___

## The Long Version
_Note: Even this "long version" is a very brief overview of this topic; your instructors will go into much more depth with you in class, covering special cases and filling in anything that is omitted here._

### The Players and the Game
* **Client** is the application used by a human user to access and interact with websites, generally a browser such as Chrome, Firefox, or Explorer/Edge.
* **Server** is a computer which hosts the programs and database that make up the website or service.
* **HTTP** is a common language that allows clients and servers to communicate with one another over the internet.

### Request/Response Cycle
**Request-Response Cycle** is the exchange of messages originating with the client, interacting with the server, and then returning to the client.

#### Request
A **request** is the message sent by the client to the server. It begins with a request line, and includes of a header and a body.
* The **request line** of an HTTP request has three parts: a verb (or method), a URL, and an HTTP-Version number. For our purposes, all HTTP requests may be assumed to use HTTP-version 1.1, so only the verb and URL matter in our work.

  The URL in an HTTP request tells the internet and the server where the request needs to go&mdash;its intended _route_. The verb tells the server what the client wants the server to do for it. Although many methods are described in the HTTP standards, only GET and POST are implemented in web browsers.

  * A **GET** request is commonly generated by clicking a link asking the server to render and provide a "view" as HTML to be displayed to the user by the browser. GET requests cover the _Read_ in the **C**reate-**R**ead-**U**pdate-**D**estroy pattern.

  * A **POST** request submits data&mdash;usually collected in an HTML form on the webpage&mdash; to the server, along with instructions about what the client is expecting the server to do with that data. POST requests must handle _Create_, _Update_, and _Destroy_ functions in a **CRUD** app. POST requests should always send the client a redirect response, and never a rendered view.

  **Remember this,** if nothing else: _**GET** requests receive **rendered views** as responses. **POST** requests receive **redirects** as responses._
* The **header** contains special codes that route the request to the server, and tell the server what the client is expecting as a response. The header consists of a series of key/value pairs, one pair per line; most of this information is important to the client browser, but is not critical to us as developers at this point.
* The **body** contains data sent from the client to the browser. The body is separated from the header by a blank line. The body of a **GET** request is empty; the body of a **POST** request contains the form data that the client expects the server to work with.

#### Response
The **response** is the message sent by the server back to the client.
* The most imporant information in the header of a response is the **status code**, a 3-digit number which provides the client with key information about the fate of its request. The first digit of the status code tells the general outcome of the request:
```
2XX: SUCCESS - the request was successfully recieved and processed.

3XX: REDIRECTION - further action must be taken by the client
(the browser, not the human user) to complete the request.

4XX: CLIENT-SIDE ERROR - the request cannot be fulfilled due to user error,
usually a request to a non-existent URL (the familiar 404 error).

5XX: SERVER-SIDE ERROR - the server cannot fulfill a valid request,
usually due to a problem in the server-side software or database.
```
* The **body** of the response contains data sent by the server to the client. Again, it is separated from the header by a blank line. The body of a **GET** request will typically contain an HTML file in the form of a string, which will be displayed by the client to the user. (In some cases, covered later in Phase 2, a response to a GET request will return a JSON object, which is a way of rendering a JavaScript object as a string, so it can be conveyed via HTTP).


## Further Reading
[HTTP (HyperText Transfer Protocol)](https://www.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html)