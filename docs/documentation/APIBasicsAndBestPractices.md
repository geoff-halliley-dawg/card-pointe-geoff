# Overview

This guide provides basic information and best practices to help you successfully integrate your application with our APIs.

The CardPointe platform provides a library of application programming interfaces (APIs) that allow you to integrate various services within your application. Each API uses a standard set of protocols to allow your application to interact with these services. Before you begin to integrate your application with our APIs, you should understand the basic concepts and the best practices for implementing them.

## API Development Basics

Before you get started, you should have a functional understanding of the following concepts:

- HTTP RESTful web methods
- JavaScript object notation (JSON)

### Understanding HTTP RESTful Web Methods

RESTful APIs provide HTTP methods to allow a client application to perform create, read, update, and delete (CRUD) functions for shared resources across the web.

> If you're new to working with RESTful APIs or HTTP methods, you can find detailed technical information and additional resources at https://restfulapi.net/http-methods/.

In general, the following HTTP request methods are used to perform these CRUD functions:

| Request Method | Function |
| -------------- | -------- |
| POST | Create a resource |
| PUT | Update a resource |
| GET | Retrieve a resource |
| DELETE | Delete a resource |

Each API supports standard HTTP request types; see the API documentation for details on the specific methods supported for each API. 

### Understanding JSON

