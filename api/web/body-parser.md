# uranite.web.body-parser

## Table of Contents

- [Imports](#imports)
- [class `BodyParserMiddleware`](#class-bodyparsermiddleware)
  - [`BodyParserMiddleware()`](#BodyParserMiddleware)
  - [`setMaxBodySize()`](#setMaxBodySize)
  - [`process()`](#process)

## Imports

- `uranite.io.syscall`
  - `stringLen`
- `uranite.web.middleware`
  - `Middleware`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## class `BodyParserMiddleware`

**Implements**: `Middleware`

Middleware that enforces a maximum request body size. If the body exceeds the configured limit, the request is rejected with a 413 (Payload Too Large) response. The default limit is 10 MB.

### Fields

| Name | Type | Access |
|------|------|--------|
| `maxBodySize` | `I64` | public |

### Methods

#### `function BodyParserMiddleware( self ) -> Void`

Create a new BodyParserMiddleware with the default maximum body size of 10 MB (10485760 bytes).

#### `function setMaxBodySize( self, I64 size ) -> BodyParserMiddleware`

Set the maximum allowed request body size.

**Parameters**:

- `size` (`I64`)
- `The maximum body size in bytes.`

**Returns**: — This BodyParserMiddleware instance for method chaining.

#### `function process( self, HttpRequest req, HttpResponse res ) -> Boolean`

Check the request body size against the configured limit. If the body exceeds the limit, a 413 JSON error response is sent and processing is halted.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request to validate.`
- `res` (`HttpResponse`)
- `The HTTP response to populate if the body is too large.`

**Returns**: — True if the body size is within the limit and processing should
continue, False if the request was rejected.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

