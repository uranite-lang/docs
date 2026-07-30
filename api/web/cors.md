# uranite.web.cors

## Table of Contents

- [Imports](#imports)
- [class `CorsMiddleware`](#class-corsmiddleware)
  - [`CorsMiddleware()`](#CorsMiddleware)
  - [`setOrigin()`](#setOrigin)
  - [`setMethods()`](#setMethods)
  - [`setHeaders()`](#setHeaders)
  - [`setCredentials()`](#setCredentials)
  - [`process()`](#process)

## Imports

- `uranite.functions.builtin`
  - `equals`
- `uranite.web.middleware`
  - `Middleware`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## class `CorsMiddleware`

**Implements**: `Middleware`

Cross-Origin Resource Sharing (CORS) middleware that adds the appropriate Access-Control-Allow-* headers to every response. Automatically handles preflight OPTIONS requests by responding with a 204 No Content status.

### Fields

| Name | Type | Access |
|------|------|--------|
| `allowOrigin` | `String` | public |
| `allowMethods` | `String` | public |
| `allowHeaders` | `String` | public |
| `allowCredentials` | `Boolean` | public |
| `maxAge` | `I64` | public |

### Methods

#### `function CorsMiddleware( self ) -> Void`

Create a new CorsMiddleware with permissive defaults: all origins allowed, standard HTTP methods, common headers, no credentials, and a 24-hour preflight cache.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setOrigin( self, String origin ) -> CorsMiddleware`

Set the allowed origin for CORS requests.

**Parameters**:

- `origin` (`String`)
- `The origin value, such as "https`

**Returns**: — This CorsMiddleware instance for method chaining.

#### `function setMethods( self, String methods ) -> CorsMiddleware`

Set the allowed HTTP methods for CORS requests.

**Parameters**:

- `methods` (`String`)
- `A comma-separated list of allowed HTTP methods.`

**Returns**: — This CorsMiddleware instance for method chaining.

#### `function setHeaders( self, String headers ) -> CorsMiddleware`

Set the allowed request headers for CORS requests.

**Parameters**:

- `headers` (`String`)
- `A comma-separated list of allowed header names.`

**Returns**: — This CorsMiddleware instance for method chaining.

#### `function setCredentials( self, Boolean allow ) -> CorsMiddleware`

Set whether credentials (cookies, authorization headers) are allowed in CORS requests.

**Parameters**:

- `allow` (`Boolean`)
- `True to include Access-Control-Allow-Credentials`

**Returns**: — This CorsMiddleware instance for method chaining.

#### `function process( self, HttpRequest req, HttpResponse res ) -> Boolean`

Add CORS headers to the response. If the request is a preflight OPTIONS request, respond immediately with 204 No Content and stop further processing.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request.`
- `res` (`HttpResponse`)
- `The HTTP response to add CORS headers to.`

**Returns**: — True if request processing should continue, False if the preflight
response has already been sent.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

