# uranite.net.http.response

## Table of Contents

- [class `HttpResponse`](#class-httpresponse)
  - [`HttpResponse()`](#HttpResponse)
  - [`isOk()`](#isOk)
  - [`isRedirect()`](#isRedirect)
  - [`isClientError()`](#isClientError)
  - [`isServerError()`](#isServerError)

## class `HttpResponse`

Parsed HTTP response containing status code, headers, and body. Populated by the HTTP parser after reading a raw response from a socket connection.

### Fields

| Name | Type | Access |
|------|------|--------|
| `statusCode` | `I64` | public |
| `statusText` | `String` | public |
| `body` | `String` | public |
| `contentType` | `String` | public |
| `contentLength` | `I64` | public |
| `version` | `String` | public |
| `rawHeaders` | `String` | public |

### Methods

#### `function HttpResponse( self, I64 statusCode, String statusText, String body ) -> Void`

Construct an HTTP response with the given status and body.

**Parameters**:

- `statusCode` (`I64`)
- `HTTP status code.`
- `statusText` (`String`)
- `HTTP reason phrase.`
- `body` (`String`)
- `Response body content.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isOk( self ) -> Boolean`

Return True if status code is in the 2xx success range.

#### `function isRedirect( self ) -> Boolean`

Return True if status code is in the 3xx redirect range.

#### `function isClientError( self ) -> Boolean`

Return True if status code is in the 4xx client error range.

#### `function isServerError( self ) -> Boolean`

Return True if status code is 500 or above.

