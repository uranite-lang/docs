# uranite.web.logger-middleware

## Table of Contents

- [Imports](#imports)
- [class `RequestLoggerMiddleware`](#class-requestloggermiddleware)
  - [`RequestLoggerMiddleware()`](#RequestLoggerMiddleware)
  - [`process()`](#process)

## Imports

- `uranite.convert.convert`
  - `intToString`
- `uranite.io.console`
  - `putsln`
- `uranite.web.middleware`
  - `Middleware`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## class `RequestLoggerMiddleware`

**Implements**: `Middleware`

Middleware that logs each HTTP request and its response status to the console.

Outputs the HTTP method, request path, and response status code for every processed request, providing basic request visibility for debugging and monitoring purposes.

### Methods

#### `function RequestLoggerMiddleware( self ) -> Void`

Construct a new RequestLoggerMiddleware instance.

#### `function process( self, HttpRequest req, HttpResponse res ) -> Boolean`

Log the request method, path, and response status code to stdout.

Prints a single line in the format "METHOD /path -> STATUS" for each request that passes through the middleware chain.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request to log.`
- `res` (`HttpResponse`)
- `The outgoing HTTP response whose status code will be logged.`

**Returns**: — Boolean:
Always returns True, allowing the middleware chain to continue.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

