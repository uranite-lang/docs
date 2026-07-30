# uranite.requests.requests

## Table of Contents

- [Imports](#imports)
- [function `request`](#function-request)
  - [`request()`](#request)
- [function `request`](#function-request)
  - [`request()`](#request)
- [function `get`](#function-get)
  - [`get()`](#get)
- [function `get`](#function-get)
- [function `post`](#function-post)
  - [`post()`](#post)
- [function `post`](#function-post)
- [function `put`](#function-put)
  - [`put()`](#put)
- [function `put`](#function-put)
- [function `del`](#function-del)
  - [`del()`](#del)
- [function `del`](#function-del)
- [function `patch`](#function-patch)
  - [`patch()`](#patch)
- [function `patch`](#function-patch)
- [function `postForm`](#function-postform)
  - [`postForm()`](#postForm)
- [function `postForm`](#function-postform)

## Imports

- `uranite.net.http.client`
  - `sendRequest`
- `uranite.net.http.request`
  - `HttpRequest`
- `uranite.net.http.response`
  - `HttpResponse`
- `uranite.requests.errors`
  - `RequestConnectionError`
  - `RequestError`
- `uranite.requests.headers`
  - `Headers`
  - `parseHeaders`
- `uranite.requests.response`
  - `Response`
- `uranite.requests.url`
  - `ParsedURL`
  - `parseURL`

## function `request`

Send an HTTP request to the specified host and port.

**Parameters**:

- `method` (`String`)
- `The HTTP method` (`e.g., "GET", "POST"`)
- `host` (`String`)
- `IPv4 address as dotted-decimal string` (`e.g., "127.0.0.1"`)
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path` (`e.g., "/api/users"`)
- `body` (`String`)
- `The request body content, or empty string for no body.`
- `contentType` (`String`)
- `The Content-Type header value, or empty string to omit.`

**Returns**: — Response:
A Response object containing the status code, body, and headers.

**Complexity**:
- Time: `O(n) where n is request + response size`
- Space: `O(n)`

### Methods

#### `function request( String method, String host, I64 port, String path, String body, String contentType ) -> Response`

Send an HTTP request to the specified host and port.

**Parameters**:

- `method` (`String`)
- `The HTTP method` (`e.g., "GET", "POST"`)
- `host` (`String`)
- `IPv4 address as dotted-decimal string` (`e.g., "127.0.0.1"`)
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path` (`e.g., "/api/users"`)
- `body` (`String`)
- `The request body content, or empty string for no body.`
- `contentType` (`String`)
- `The Content-Type header value, or empty string to omit.`

**Returns**: — Response:
A Response object containing the status code, body, and headers.

**Complexity**:
- Time: `O(n) where n is request + response size`
- Space: `O(n)`

## function `request`

Send an HTTP request using a URL string.

Parses the URL into host, port, and path components, then delegates to the host-based request function.

**Parameters**:

- `method` (`String`)
- `The HTTP method` (`e.g., "GET", "POST"`)
- `url` (`String`)
- `The full URL` (`e.g., "http://127.0.0.1:8080/api/users"`)
- `body` (`String`)
- `The request body content, or empty string for no body.`
- `contentType` (`String`)
- `The Content-Type header value, or empty string to omit.`

**Returns**: — Response:
A Response object containing the status code, body, and headers.

**Complexity**:
- Time: `O(n) where n is URL + request + response size`
- Space: `O(n)`

### Methods

#### `function request( String method, String url, String body, String contentType ) -> Response`

Send an HTTP request using a URL string.

Parses the URL into host, port, and path components, then delegates to the host-based request function.

**Parameters**:

- `method` (`String`)
- `The HTTP method` (`e.g., "GET", "POST"`)
- `url` (`String`)
- `The full URL` (`e.g., "http://127.0.0.1:8080/api/users"`)
- `body` (`String`)
- `The request body content, or empty string for no body.`
- `contentType` (`String`)
- `The Content-Type header value, or empty string to omit.`

**Returns**: — Response:
A Response object containing the status code, body, and headers.

**Complexity**:
- Time: `O(n) where n is URL + request + response size`
- Space: `O(n)`

## function `get`

Send an HTTP GET request to the given URL.

**Parameters**:

- `url` (`String`)
- `The full URL` (`e.g., "http://127.0.0.1:8080/api/users"`)

**Returns**: `Response` — The server's response.

### Methods

#### `function get( String url ) -> Response`

Send an HTTP GET request to the given URL.

**Parameters**:

- `url` (`String`)
- `The full URL` (`e.g., "http://127.0.0.1:8080/api/users"`)

**Returns**: `Response` — The server's response.

## function `get`

Send an HTTP GET request using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`

**Returns**: `Response` — The server's response.

### Methods

#### `function get( String host, I64 port, String path ) -> Response`

Send an HTTP GET request using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`

**Returns**: `Response` — The server's response.

## function `post`

Send an HTTP POST request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

### Methods

#### `function post( String url, String body ) -> Response`

Send an HTTP POST request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

## function `post`

Send an HTTP POST request with a JSON body using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

### Methods

#### `function post( String host, I64 port, String path, String body ) -> Response`

Send an HTTP POST request with a JSON body using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

## function `put`

Send an HTTP PUT request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

### Methods

#### `function put( String url, String body ) -> Response`

Send an HTTP PUT request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

## function `put`

Send an HTTP PUT request with a JSON body using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

### Methods

#### `function put( String host, I64 port, String path, String body ) -> Response`

Send an HTTP PUT request with a JSON body using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

## function `del`

Send an HTTP DELETE request.

**Parameters**:

- `url` (`String`)
- `The full URL.`

**Returns**: `Response` — The server's response.

### Methods

#### `function del( String url ) -> Response`

Send an HTTP DELETE request.

**Parameters**:

- `url` (`String`)
- `The full URL.`

**Returns**: `Response` — The server's response.

## function `del`

Send an HTTP DELETE request using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`

**Returns**: `Response` — The server's response.

### Methods

#### `function del( String host, I64 port, String path ) -> Response`

Send an HTTP DELETE request using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`

**Returns**: `Response` — The server's response.

## function `patch`

Send an HTTP PATCH request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

### Methods

#### `function patch( String url, String body ) -> Response`

Send an HTTP PATCH request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

## function `patch`

Send an HTTP PATCH request with a JSON body using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

### Methods

#### `function patch( String host, I64 port, String path, String body ) -> Response`

Send an HTTP PATCH request with a JSON body using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

## function `postForm`

Send an HTTP POST with URL-encoded form data.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The URL-encoded form data.`

**Returns**: `Response` — The server's response.

### Methods

#### `function postForm( String url, String body ) -> Response`

Send an HTTP POST with URL-encoded form data.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The URL-encoded form data.`

**Returns**: `Response` — The server's response.

## function `postForm`

Send an HTTP POST with URL-encoded form data using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`
- `body` (`String`)
- `The URL-encoded form data.`

**Returns**: — Response:
The server's response.

**Complexity**:
- Time: `O(n) where n is request + response size`
- Space: `O(n)`

### Methods

#### `function postForm( String host, I64 port, String path, String body ) -> Response`

Send an HTTP POST with URL-encoded form data using host and port directly.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string.`
- `port` (`I64`)
- `The target server port number.`
- `path` (`String`)
- `The request URI path.`
- `body` (`String`)
- `The URL-encoded form data.`

**Returns**: — Response:
The server's response.

**Complexity**:
- Time: `O(n) where n is request + response size`
- Space: `O(n)`

