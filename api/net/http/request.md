# uranite.net.http.request

## Table of Contents

- [Imports](#imports)
- [class `HttpRequest`](#class-httprequest)
  - [`HttpRequest()`](#HttpRequest)
  - [`setHost()`](#setHost)
  - [`setBody()`](#setBody)
  - [`setContentType()`](#setContentType)
  - [`setUserAgent()`](#setUserAgent)
  - [`setAccept()`](#setAccept)
  - [`setConnection()`](#setConnection)
  - [`addRawHeaders()`](#addRawHeaders)
  - [`serialize()`](#serialize)

## Imports

- `uranite.io.syscall`
  - `stringLen`
- `uranite.string.builder`
  - `StringBuilder`

## class `HttpRequest`

HTTP request builder with fluent setter API. Constructs an HTTP/1.1 request with method, path, headers, and optional body. Call serialize() to produce the raw HTTP message ready to send over a socket.

### Fields

| Name | Type | Access |
|------|------|--------|
| `method` | `String` | public |
| `path` | `String` | public |
| `version` | `String` | public |
| `host` | `String` | public |
| `body` | `String` | public |
| `contentType` | `String` | public |
| `userAgent` | `String` | public |
| `accept` | `String` | public |
| `connection` | `String` | public |
| `extraHeaders` | `String` | public |

### Methods

#### `function HttpRequest( self, String method, String path ) -> Void`

Construct a new HTTP request with the given method and path. All other fields are initialized to sensible defaults.

**Parameters**:

- `method` (`String`)
- `HTTP method verb.`
- `path` (`String`)
- `Request target path.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setHost( self, String host ) -> HttpRequest`

Set the Host header. Returns self for chaining.

#### `function setBody( self, String body ) -> HttpRequest`

Set the request body. Returns self for chaining.

#### `function setContentType( self, String ct ) -> HttpRequest`

Set the Content-Type header. Returns self for chaining.

#### `function setUserAgent( self, String ua ) -> HttpRequest`

Set the User-Agent header. Returns self for chaining.

#### `function setAccept( self, String acc ) -> HttpRequest`

Set the Accept header. Returns self for chaining.

#### `function setConnection( self, String conn ) -> HttpRequest`

Set the Connection header. Returns self for chaining.

#### `function addRawHeaders( self, String rawBlock ) -> HttpRequest`

Append raw header lines (already formatted as "Name: Value\\r\\n"). Returns self for chaining.

#### `function serialize( self ) -> String`

Serialize the request into a raw HTTP message string ready for transmission over a TCP socket. Includes request line, headers, and body with appropriate Content-Length.

**Returns**: — The complete HTTP request as a string.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

