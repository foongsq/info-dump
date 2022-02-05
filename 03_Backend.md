---
layout: page
title: Backend
permalink: /backend/
---

* Table of Contents
{:toc}

--------------------------------------------------------------------------------------------------------------------

# API Development

## Rest API

stands for **Representational State Transfer**
* **Client-server architecture:** and communicate through HTTP
* **Stateless communication:** each request is independent of one another
* **Cacheable data:** Make data cacheable by including expiry time in response, or which data should not be cached. This is so that we can help to increase efficiency in our application.
* **Uniform interface:** Allow standardized means of communicating between server and client, usually using HTTP with URI resources, CRUD, and JSON. (This is to decouple client from server).
* **Layered system:** separates the different types of servers (eg. one for security, one for load balancing etc) 
* **Code on demand:** ?

### **Best practice: Use the resource name as the api route name**

Eg. For a route that creates a new user in the database, use the route `/user` instead of `/createUser`.

### **Best practice: For POST or DELETE methods, return the object created or deleted in the response**

This is to make it easier for frontend developers to debug.

## RPC API

- Focuses on actions instead of resources (more intuitive to us)
- Have lightweight payloads because they are action oriented, which leads to higher performance
- More difficult discovery
- Harder to standardize, and might lead to a lot of RPCs.

## GraphQL API

- Easier to do versioning
- Added complexity
- Exposes only 1 entry point, client defines how data should be received. (give a lot of flexibility)
- Should not be used for simple things


# Python API development

## `requirements.txt` file to make it easier for other developers to install project dependencies

When I was installing dependencies for the project, I wondered how I would have to repeat this process of installing the dependencies one by one again if I were to set up this project again. I looked online for a solution, and found that people often used this `requirements.txt` file to solve this issue.

## Use of virtual environment

I chose to use a python virtual environment for the API section of this project to prevent the packages/libraries installed in this project to affect the packages/libraries used in other projects on my computer. This is a best practice when working with Python based projects especially.

## Middleware in Node.js VS Decorators in Flask

It's like a middleman between 2 software applications, and provides services to them beyond those available from OS. 

Middleware functions have access to request, response and next function in request-response cycle. Middleware function is like the functions we define after the routes in flask (Except its in nodejs).

Middleware classes in flask seem to be a different thing. Flask middleware is a WSGI middleware which operates by wrapping the Flask application instance.

Use route decorators in flask instead. route decorators decorate a view function to register it with the given url rule and options.

## Web Server Gateway Interface (WSGI)

Used to forward requests from a web server (such as Apache or nginx) to a backend python web application or framework.

## Use of Flask Blueprints to modularize backend code

[Useful explanation of blueprints](https://realpython.com/flask-blueprint/)

## Using python decorators to reduce code duplication for authentication

I realised that there was a lot of code duplication in my api in which I retrieved the jwt token from the cookies for every path and process to get user etc. So I went to search online and found that I could use python decorators `@auth_required` to abstract out all that logic into 1 reusable function. This made my code a lot more succint and readable.

# Security

## `.env` file to store sensitive information such as private keys

I knew it was unsafe to put private keys and sensitive information on a git repository, so I looked for a way to store that information without commiting it directly to Github. I found that it was common practice to use a `.env` file for this purpose.

## Cross Origin Resource Sharing (CORS)
A HTTP header mechanism that allows a server to indicate which origins other than its own that a browser can load resources from. Browsers usually make a preflight request to the server to check that the server will permit the request.

## Exposing MongoDB ObjectId in url query

Originally, I was concerned with exposing the mongodb object id in url query (when building endpoint for deleting store items), but upon some reading, I found that the object id is based upon the time of creation of the document, hence the information is not sensitive. Also, [someone on quora](https://www.quora.com/What-if-any-are-the-security-risks-associated-with-exposing-a-MongoID-string-in-a-URL) mentioned that trying to hide the object id is a kind of "security through obscurity" practice, which we should not be depending on. We should instead focus on access control to the database.

## Storing passwords in plaintext

Storing passwords in plaintext is really bad and violates our users privacy as anyone who has access to the database can see the passwords. Hence, we have to hash the passwords before storing it into our database. I chose to use the passlib library which has no known weaknesses at the time of writing. (Use `.verify()` method instead of `==` to compare whether password matches to prevent timing attacks)

## Using JWT token for authentication

When someone logs in successfully, server will sign a jwt token and send it to the client. Client can then send that jwt token together with requests so that they can get access to routes that require authentication. Server will verify that token is valid, then send data/resources back.

A jwt token is actually made of header, payload and secret. Payload is somewhat of a hash of certain info we put in, eg user id, admin role etc... and then it is signed with a secret key that only the server should know. Signature prevents tampering because when attacker changes role, signature will no longer match, and they need the secret key in order to sign it properly.

The reason why we need JWT tokens is to prevent us from having to collect username and password every time someone wants to request for a resource. We work around it by using a session id. but session id doesnt scale because if we have multiple servers behind a load balancer, the different servers wont know what session ids other servers issued.

[Good explanation about JWT tokens](https://medium.com/swlh/why-do-we-need-the-json-web-token-jwt-in-the-modern-web-8490a7284482)

## Never store jwt tokens inside localStorage of browser

This is because localStorage or sessionStorage is accessible by any script inside your page, so a XSS attack can give a hacker access to your token, and compromise authenticity. Instead, store it in a httpOnly cookie, which is a special kind of cookie that is only sent in http reqests to the server, and it is **never accessible** from javascript running in the browser.

[JWT best practices](https://blog.logrocket.com/jwt-authentication-best-practices/)

## Cookies

A cookie is a piece of data that your web server can set, that will be then stored by the user's web browser and sent back to the server on any future requests that browser makes to the same server as long as the cookie is valid and applicable to the request being made. Cookies are a **state management** mechanism for HTTP.

Need to use a HTTP proxy also because since we are now sending the token in a cookie, we need to run the 2 apps on the same port. (because cookies can only go to origins from which they came)

[Cookie explanation](https://stackoverflow.com/questions/39810741/how-to-store-a-jwt-token-inside-an-http-only-cookie)

[Cookie implementation](https://medium.com/@ryanchenkie_40935/react-authentication-how-to-store-jwt-in-a-cookie-346519310e81)

[Set up HTTP Proxy for Vue](https://medium.com/bb-tutorials-and-thoughts/vue-js-how-to-proxy-to-backend-server-a562bad965eb)

# Data

## base64 encoding

base64 is a binary to text encoding scheme that represents binary data. `64` comes from the `radix-64 representation`. Binary to text encoding schemes are designed to carry data stored in binary formats across channels that only reliably support text content. In my project, I used base64 encoding to convert image files into text so that they can be passed from backend to frontend in JSON format.

## Binary large object (blob)

It is a collection of binary data stored as a single entity, usually is images, audio, or binary code. 

# Database

<!-- ## Structured VS Unstructured Data

## ACID Properties

## CAP theorem

## SQL vs NoSQL databases

## Replication

## Horizontal vs Vertical Scaling

## Data Sharding

## Caching
### Caching techniques
### Distributed caching
### CDNs -->

## MongoDB Schema Design
- For relational, focus on: Don't duplicate data
- For NoSQL, no rules, no process, no algorithm, consider how to store data and query performance. (Focus on designing a schema uniquely suited to your application)
- Embedding vs Referencing. Embedding is storing object within another object (same as join)
  - Pros of embedding: retrieve all data in a single query, can avoid expensive joins and can update all data with a single atomic operation.
  - Cons of embedding: large docs means more overhead, if you dont need all data at once, dont embed. 16MB document size limit
  - Pros of referencing: smaller docs, less likely to reach 16mb limit, no duplication of data, infrequently accessed data not accessed on every query
  - Cons of referencing: More queries required to retrieve all data
- Favor embedding unless there is a compelling reason not to. (Needing to access an object on its own is a compelling reason not to embed it)
- Avoid joins (embedding?)
- Arrays should not grow without bound
- Relations in MongoDB
  - One to one: Use key value pairs
  - One to few: Prefer embedding
  - One to many: Prefer referencing
  - One to squillions: Prefer referencing

## MongoDB: Embedding VS Referencing (in the context of money game)

Pros:
- Can retrieve all data in a single query
- Shouldnt have too many items in the inventory
- We wont have to retrieve items 1 by 1 so there is not much incentive to use referencing.

Cons: 
- might exceed 16MB document limit (especially since we are storing images inside the JSON too)
  - 0.2KB for a document with imgUrl
  - about 50KB for a document with base64Img, depends on size of image I think
  - Can get around this by compressing the image before storing

<!-- ## Relational databases -->

