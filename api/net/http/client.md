# uranite.net.http.client

## Table of Contents

- [Imports](#imports)
- [const `RECV_BUF_SIZE`](#const-recv-buf-size)
- [function `readFull`](#function-readfull)
  - [`readFull()`](#readFull)
- [function `sendRequest`](#function-sendrequest)
  - [`sendRequest()`](#sendRequest)
- [function `sendRequest`](#function-sendrequest)
  - [`sendRequest()`](#sendRequest)
- [function `sendRequestRaw`](#function-sendrequestraw)
- [function `get`](#function-get)
  - [`get()`](#get)
- [function `get`](#function-get)
  - [`get()`](#get)
- [function `post`](#function-post)
  - [`post()`](#post)
- [function `post`](#function-post)
  - [`post()`](#post)
- [function `put`](#function-put)
  - [`put()`](#put)
- [function `put`](#function-put)
  - [`put()`](#put)
- [function `del`](#function-del)
  - [`del()`](#del)
- [function `del`](#function-del)
  - [`del()`](#del)
- [function `patch`](#function-patch)
  - [`patch()`](#patch)
- [function `patch`](#function-patch)
  - [`patch()`](#patch)
- [function `head`](#function-head)
  - [`head()`](#head)
- [function `head`](#function-head)
  - [`head()`](#head)
- [function `options`](#function-options)
  - [`options()`](#options)
- [function `options`](#function-options)
  - [`options()`](#options)

## Imports

- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`
  - `realloc`
- `uranite.net.address`
  - `HostAddress`
  - `parseHostPort`
  - `parseIpv4`
- `uranite.net.http.errors`
  - `HttpConnectionError`
- `uranite.net.http.parser`
  - `parseResponse`
- `uranite.net.http.request`
  - `HttpRequest`
- `uranite.net.http.response`
  - `HttpResponse`
- `uranite.net.socket`
  - `INADDR_LOOPBACK`
  - `ipToInt`
  - `sendString`
  - `socketClose`
  - `socketRecv`
  - `tcpConnect`
- `uranite.string.builder`
  - `StringBuilder`

## const `RECV_BUF_SIZE`

Size in bytes of each individual socket receive chunk. 

## function `readFull`

Read all available data from a socket until the remote end closes the connection. Dynamically grows the receive buffer as needed.

**Parameters**:

- `fd` (`I64`)
- `The connected socket file descriptor to read from.`

**Returns**: — String:
The complete response data as a null-terminated string.

**Complexity**:
- Time: `O(n) where n is the total response size`
- Space: `O(n), buffer doubles on overflow`

### Methods

#### `function readFull( I64 fd ) -> String`

Read all available data from a socket until the remote end closes the connection. Dynamically grows the receive buffer as needed.

**Parameters**:

- `fd` (`I64`)
- `The connected socket file descriptor to read from.`

**Returns**: — String:
The complete response data as a null-terminated string.

**Complexity**:
- Time: `O(n) where n is the total response size`
- Space: `O(n), buffer doubles on overflow`

## function `sendRequest`

Send an HTTP request to the specified "host:port" address and return the parsed response. The address is parsed internally via parseHostPort.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host:port" format` (`e.g., "127.0.0.1:8080"`)
- `request` (`HttpRequest`)
- `The HTTP request to send.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function sendRequest( String address, HttpRequest request ) -> HttpResponse`

Send an HTTP request to the specified "host:port" address and return the parsed response. The address is parsed internally via parseHostPort.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host:port" format` (`e.g., "127.0.0.1:8080"`)
- `request` (`HttpRequest`)
- `The HTTP request to send.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `sendRequest`

Send an HTTP request to the specified host and return the parsed response. Opens a TCP connection, sends the serialized request, reads the full response, and parses it into an HttpResponse.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string` (`e.g., "127.0.0.1"`)
- `port` (`I64`)
- `TCP port number to connect to.`
- `request` (`HttpRequest`)
- `The HTTP request to send.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function sendRequest( String host, I64 port, HttpRequest request ) -> HttpResponse`

Send an HTTP request to the specified host and return the parsed response. Opens a TCP connection, sends the serialized request, reads the full response, and parses it into an HttpResponse.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string` (`e.g., "127.0.0.1"`)
- `port` (`I64`)
- `TCP port number to connect to.`
- `request` (`HttpRequest`)
- `The HTTP request to send.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `sendRequestRaw`

Send an HTTP request using a pre-resolved packed IPv4 address. Internal use — prefer sendRequest(String, I64, HttpRequest).

**Parameters**:

- `ipAddress` (`I64`)
- `IPv4 address as a 32-bit integer in network byte order.`
- `port` (`I64`)
- `TCP port number to connect to.`
- `request` (`HttpRequest`)
- `The HTTP request to send.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function sendRequestRaw( I64 ipAddress, I64 port, HttpRequest request ) -> HttpResponse`

Send an HTTP request using a pre-resolved packed IPv4 address. Internal use — prefer sendRequest(String, I64, HttpRequest).

**Parameters**:

- `ipAddress` (`I64`)
- `IPv4 address as a 32-bit integer in network byte order.`
- `port` (`I64`)
- `TCP port number to connect to.`
- `request` (`HttpRequest`)
- `The HTTP request to send.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `get`

Send an HTTP GET request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host:port" format` (`e.g., "127.0.0.1:8080"`)
- `path` (`String`)
- `Request target path` (`e.g. "/api/users"`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function get( String address, String path ) -> HttpResponse`

Send an HTTP GET request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host:port" format` (`e.g., "127.0.0.1:8080"`)
- `path` (`String`)
- `Request target path` (`e.g. "/api/users"`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `get`

Send an HTTP GET request and return the parsed response.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path` (`e.g. "/api/users"`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function get( String host, I64 port, String path ) -> HttpResponse`

Send an HTTP GET request and return the parsed response.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path` (`e.g. "/api/users"`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `post`

Send an HTTP POST request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function post( String address, String path, String body, String contentType ) -> HttpResponse`

Send an HTTP POST request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `post`

Send an HTTP POST request with the given body and content type.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header` (`e.g. "application/json"`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function post( String host, I64 port, String path, String body, String contentType ) -> HttpResponse`

Send an HTTP POST request with the given body and content type.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header` (`e.g. "application/json"`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `put`

Send an HTTP PUT request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function put( String address, String path, String body, String contentType ) -> HttpResponse`

Send an HTTP PUT request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `put`

Send an HTTP PUT request with the given body and content type.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function put( String host, I64 port, String path, String body, String contentType ) -> HttpResponse`

Send an HTTP PUT request with the given body and content type.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `del`

Send an HTTP DELETE request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function del( String address, String path ) -> HttpResponse`

Send an HTTP DELETE request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `del`

Send an HTTP DELETE request and return the parsed response.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function del( String host, I64 port, String path ) -> HttpResponse`

Send an HTTP DELETE request and return the parsed response.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `patch`

Send an HTTP PATCH request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function patch( String address, String path, String body, String contentType ) -> HttpResponse`

Send an HTTP PATCH request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `patch`

Send an HTTP PATCH request with the given body and content type.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function patch( String host, I64 port, String path, String body, String contentType ) -> HttpResponse`

Send an HTTP PATCH request with the given body and content type.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`
- `body` (`String`)
- `Request body content.`
- `contentType` (`String`)
- `Value for the Content-Type header.`

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `head`

Send an HTTP HEAD request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`

**Returns**: — HttpResponse:
The parsed HTTP response (body empty).

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function head( String address, String path ) -> HttpResponse`

Send an HTTP HEAD request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path.`

**Returns**: — HttpResponse:
The parsed HTTP response (body empty).

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `head`

Send an HTTP HEAD request. The response body will be empty per HTTP specification, but headers are fully populated.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`

**Returns**: — HttpResponse:
The parsed HTTP response (body empty).

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function head( String host, I64 port, String path ) -> HttpResponse`

Send an HTTP HEAD request. The response body will be empty per HTTP specification, but headers are fully populated.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path.`

**Returns**: — HttpResponse:
The parsed HTTP response (body empty).

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `options`

Send an HTTP OPTIONS request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path` (`use "*" for server-wide options`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function options( String address, String path ) -> HttpResponse`

Send an HTTP OPTIONS request to the specified "host:port" address.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host`
- `path` (`String`)
- `Request target path` (`use "*" for server-wide options`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

## function `options`

Send an HTTP OPTIONS request to discover supported methods and capabilities of the server resource.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path` (`use "*" for server-wide options`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

### Methods

#### `function options( String host, I64 port, String path ) -> HttpResponse`

Send an HTTP OPTIONS request to discover supported methods and capabilities of the server resource.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `TCP port number.`
- `path` (`String`)
- `Request target path` (`use "*" for server-wide options`)

**Returns**: — HttpResponse:
The parsed HTTP response from the server.

**Complexity**:
- Time: `O(n) where n is response size`
- Space: `O(n)`

