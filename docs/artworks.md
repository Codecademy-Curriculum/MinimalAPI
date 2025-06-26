# List of Art Requests

1. [OAuth Roles](#OAuth-Roles)
2. [Access Token Response](#Access-Token-Response)
3. [Grant Types](#Grant-Types)
4. [HTTP Requests](#HTTP-Requests)
5. [Structure of HTTP](#Structure-of-HTTP)
6. [TCP vs UDP](#TCP-vs-UDP)
7. [Anatomy of a URL](#Anatomy-of-a-URL)
8. [HTTP with Database](#HTTP-with-Database)
9. [Microservice](#Microservice)

## OAuth Roles

**For**: OAuth 2.0 Lesson, Introduction to OAuth exercise

**Location**: Workspace (right half of LE)

**Details**:

Should describe the relationship between the four below roles:

- Resource Owner is the _user_ who authorizes an _application_ to access an account
- Resource Server is the API server that accepts access tokens and verifies them as valid
- Authorization server issues access tokens
- Client is the application that requests the access token

**References**:

![example diagram of oauth roles](../lessons/outlines/oauth/roles.png)

## Access Token Response

**For**: OAuth 2.0 Lesson, OAuth Tokens exercise

**Location**: Workspace (right half of LE)

**Details**:

An image of below code:

```HTTP
HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
Content-Length: 154
ETag: W/"9a-eSnSBQylEE0P1rE3vw/nZ0RVczc"
Date: Wed, 19 May 2021 16:01:47 GMT
Connection: close

{
  "accessToken": "4b2a2d60cecc37ceff6065027210be53b6f7a32b",
  "accessTokenExpiresAt": "2021-05-19T17:01:47.143Z",
  "client": {
    "id": "secretapplication"
  },
  "user": {}
}
```

## Grant Types

**For**: OAuth 2.0 Lesson, Grant Types exercise

**Location**: Workspace (right half of LE)

**Details**:

Should describe the relationship between the four below grant types:

- Client Credential Grant
- Authorization Code Grant
- Implicit Grant
- Resource Owner Password Credential

**References**:

![example diagram of grant types](../lessons/outlines/oauth/grants.png)

## HTTP Requests

**For**: HTTP Lesson, Introduction to HTTP exercise

**Location**: Workspace (right half of LE)

**Details**:

Show a general diagram of HTTP requests moving between different entities that comprise the web (ie: clients such as browsers, servers, etc.)

**References**:

![example of http requests diagram](https://csharpcorner.azureedge.net/UploadFile/deveshomar/debugging-http-requests-and-http-response/Images/Client-Serve-Request-Response-diagram.jpg)

## Structure of HTTP

**For**: HTTP Lesson, The Structure of HTTP exercise

**Location**: Workspace (right half of LE)

**Details**:

Include two diagrams, one for the structure of an HTTP request and another for structure of an HTTP response, side by side.

**References**:

![example diagram of HTTP request](https://images.zapier.com/storage/photos/4717d012f26dc6a4928e0d025102af7f.png?format=jpg)

![Example diagram of HTTP Response](https://www3.ntu.edu.sg/home/ehchua/programming/webprogramming/images/HTTP_ResponseMessageExample.png)

## TCP vs UDP

**For**: HTTP Lesson, The Movement of HTTP exercise

**Location**: Workspace (right half of LE)

**Details**:

The diagram should compare the two communication protocols: TCP and UDP.

**References**:

![example diagram of tcp vs udp](https://www.educative.io/api/edpresso/shot/5669375870763008/image/5080565785034752)

---

ADDITIONAL ARTWORKS

## Anatomy of a URL

**For**: HTTP Lesson, The Anatomy of the URL exercise

**Location**: In narrative (left side of LE)

**Details**:

This diagram labels different parts of an URL. The URL for the final diagram should NOT be same as the diagram below.

**References**:

![Anatomy of a URL](https://help.marketruler.com/images/9/97/URL-Parts.png)

## HTTP with Database

**For**: HTTP Lesson, Interactive with a Database exercise

**Location**: In narrative (left side of LE)

**Details**:

The diagram can look very similar to one below.

**References**:

![Server to database diagram](https://www.progress.com/documentation/sitefinity-cms/sf-images/default-source/default-album/architecture-diagram-8.jpg?sfvrsn=0)

## Microservice

**For**: HTTP Lesson, Interacting with another Back-end API exercise

**Location**: In narrative (left side of LE)

**Details**:

The diagram can look very similar to one below. It should compare the two architectures side-by-side, with the focus being on the Microservices Architecture. The placement of microservices need not be same as below.

**References**:

![microservice diagram](https://dzone.com/storage/temp/5302608-1.png)
