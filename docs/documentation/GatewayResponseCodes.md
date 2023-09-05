# Overview

This page describes the responses you may receive when sending CardPointe Gateway API requests, including HTTP errors and a detailed reference for authorization response codes. 

# HTTP Errors

HTTP errors are returned when the client (for example, the browser) encounters an issue communicating with the host server. 

| Error | Description |
| --- | --- 
| 400	| The server was unable to decipher the request because of invalid syntax. Update the request before repeating.
| 401	| The credentials passed in the request were not accepted by the server. Update the request before repeating.
| 404 | The endpoint url path specified in the request was incorrect.
| 500	| The host server encountered an issue and was unable to send a response.

# Response Codes

