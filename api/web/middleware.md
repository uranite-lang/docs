# uranite.web.middleware

## Table of Contents

- [Imports](#imports)
- [interface `Middleware`](#interface-middleware)
  - [`process()`](#process)

## Imports

- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## interface `Middleware`

Interface for HTTP middleware components in the request/response processing pipeline.

Implementations intercept HTTP requests and responses before or after route handling, enabling cross-cutting concerns such as logging, authentication, CORS headers, and request transformation.

### Methods

#### `function process( self, HttpRequest req, HttpResponse res ) -> Boolean`

Process an HTTP request/response pair in the middleware chain.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request to inspect or modify.`
- `res` (`HttpResponse`)
- `The outgoing HTTP response to inspect or modify.`

**Returns**: `Boolean` — True to continue processing the middleware chain, False to halt further processing.

