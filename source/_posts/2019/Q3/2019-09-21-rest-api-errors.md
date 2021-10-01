---
title: REST API Error Handling Best Practices
excerpt: REST API Error Handling Best Practices
date: 2019-09-21 13:28:17
tags:
  - REST
categories: [Programming, API]
---

# REST API Error Handling Best Practices

# References
+ http://jsonapi.org/format/
+ http://json-schema.org
+ http://www.jsonrpc.org/specification
+ https://apigee.com/about/blog/technology/restful-api-design-what-about-errors
+ https://blog.jayway.com/2014/10/19/spring-boot-error-responses/
+ https://docs.microsoft.com/en-us/rest/api/storageservices/common-rest-api-error-codes
+ https://google.github.io/styleguide/jsoncstyleguide.xml
+ https://nordicapis.com/best-practices-api-error-handling/
+ https://tools.ietf.org/html/rfc7807
+ https://www.petrikainulainen.net/programming/spring-framework/spring-from-the-trenches-adding-validation-to-a-rest-api/
+ https://www.restcase.com

# HTTP status
+ https://en.wikipedia.org/wiki/List_of_HTTP_status_codes
+ 200 OK
+ 201 Created
+ 202 Accepted
+ 204 No Content
+ 301 Moved Permanently
+ 304 Not Modified
+ 307 Temporary Redirect
+ 308 Permanent Redirect
+ 400 Bad Request
+ 401 Unauthorized
+ 403 Forbidden
+ 404 NOT FOUND
+ 405 Method Not Allowed
+ 409 Conflict
+ 410 Gone
+ 412 Precondition Failed
+ 429 Too Many Requests
+ 500 Internal Server Error
+ 501 Not Implemented
+ 502 Bad Gateway
+ 503 Service Unavailable
+ 508 Loop Detected

# Sample
```
   HTTP/1.1 403 Forbidden
   Content-Type: application/problem+json
   Content-Language: en

   {
    "timestamp":"2019-09-21 13:01:59.667+08:00",
    "path":"/api/books/12345"
    "status" : 403,
    "code" : 123,
    "error":"NoHostAvailable",
    "message": "Your current balance is 30, but that costs 50."
   }
```
