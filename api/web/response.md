# uranite.web.response

## Table of Contents

- [Imports](#imports)
- [class `HttpResponse`](#class-httpresponse)
  - [`HttpResponse()`](#HttpResponse)
  - [`status()`](#status)
  - [`header()`](#header)
  - [`contentType()`](#contentType)
  - [`json()`](#json)
  - [`text()`](#text)
  - [`html()`](#html)
  - [`redirect()`](#redirect)
  - [`cookie()`](#cookie)
  - [`serialize()`](#serialize)
- [function `statusCodeText`](#function-statuscodetext)
  - [`statusCodeText()`](#statusCodeText)

## Imports

- `uranite.convert.convert`
  - `intToString`
- `uranite.io.syscall`
  - `stringLen`
- `uranite.string.builder`
  - `StringBuilder`

## class `HttpResponse`

Represents an outgoing HTTP response with a fluent builder API for setting status codes, headers, content type, body, redirects, and cookies.

The response is serialized into a complete HTTP/1.1 response string via the serialize method for transmission over a socket.

### Fields

| Name | Type | Access |
|------|------|--------|
| `statusCode` | `I64` | public |
| `statusText` | `String` | public |
| `body` | `String` | public |
| `contentTypeValue` | `String` | public |
| `sent` | `Boolean` | public |
| `headerBuilder` | `StringBuilder` | public |

### Methods

#### `function HttpResponse( self ) -> Void`

Construct a new HttpResponse with default values.

Defaults to status 200 OK with text/plain content type and an empty body.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function status( self, I64 code ) -> HttpResponse`

Set the HTTP status code and automatically resolve its reason phrase.

**Parameters**:

- `code` (`I64`)
- `The HTTP status code to set` (`e.g., 200, 404, 500`)

**Returns**: `HttpResponse` — Self, for method chaining.

#### `function header( self, String name, String value ) -> HttpResponse`

Add a custom HTTP response header.

**Parameters**:

- `name` (`String`)
- `The header name` (`e.g., "X-Request-Id"`)
- `value` (`String`)
- `The header value.`

**Returns**: — HttpResponse:
Self, for method chaining.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function contentType( self, String ct ) -> HttpResponse`

Set the Content-Type header for this response.

**Parameters**:

- `ct` (`String`)
- `The MIME type string` (`e.g., "application/json", "text/html"`)

**Returns**: `HttpResponse` — Self, for method chaining.

#### `function json( self, String jsonBody ) -> Void`

Set the response body as JSON content.

Sets the Content-Type to "application/json" and marks the response as sent.

**Parameters**:

- `jsonBody` (`String`)
- `The JSON-encoded response body string.`

#### `function text( self, String textBody ) -> Void`

Set the response body as plain text content.

Sets the Content-Type to "text/plain" and marks the response as sent.

**Parameters**:

- `textBody` (`String`)
- `The plain text response body string.`

#### `function html( self, String htmlBody ) -> Void`

Set the response body as HTML content.

Sets the Content-Type to "text/html" and marks the response as sent.

**Parameters**:

- `htmlBody` (`String`)
- `The HTML response body string.`

#### `function redirect( self, String url ) -> Void`

Issue an HTTP 302 redirect to the specified URL.

Sets the status to 302 Found, adds a Location header, clears the body, and marks the response as sent.

**Parameters**:

- `url` (`String`)
- `The target URL to redirect the client to.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function cookie( self, String name, String value ) -> HttpResponse`

Set a cookie on the response via the Set-Cookie header.

The cookie is set with Path=/ by default.

**Parameters**:

- `name` (`String`)
- `The cookie name.`
- `value` (`String`)
- `The cookie value.`

**Returns**: `HttpResponse` — Self, for method chaining.

#### `function serialize( self ) -> String`

Serialize the response into a complete HTTP/1.1 response string.

Builds the status line, Content-Type, Content-Length, Connection: close, any custom headers, and the body into a single string ready for transmission over a socket.

**Returns**: — String:
The fully formatted HTTP/1.1 response string.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `statusCodeText`

Map an HTTP status code to its standard reason phrase.

Supports common status codes: 200, 201, 204, 301, 302, 304, 400, 401, 403, 404, 405, 409, 500, 502, 503.

**Parameters**:

- `code` (`I64`)
- `The HTTP status code.`

**Returns**: — String:
The corresponding reason phrase, or "Unknown" for unrecognized codes.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function statusCodeText( I64 code ) -> String`

Map an HTTP status code to its standard reason phrase.

Supports common status codes: 200, 201, 204, 301, 302, 304, 400, 401, 403, 404, 405, 409, 500, 502, 503.

**Parameters**:

- `code` (`I64`)
- `The HTTP status code.`

**Returns**: — String:
The corresponding reason phrase, or "Unknown" for unrecognized codes.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

