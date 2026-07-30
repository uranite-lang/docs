# uranite.web.filter

## Table of Contents

- [Imports](#imports)
- [interface `RequestFilter`](#interface-requestfilter)
  - [`filterRequest()`](#filterRequest)
- [interface `ResponseFilter`](#interface-responsefilter)
  - [`filterResponse()`](#filterResponse)

## Imports

- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## interface `RequestFilter`

Interface for filtering incoming HTTP requests before they reach the route handler. Implementations can inspect, modify, or reject requests during pre-processing.

### Methods

#### `function filterRequest( self, HttpRequest req ) -> Boolean`

Filter an incoming HTTP request before it reaches the route handler.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request to filter.`

**Returns**: — True if the request should proceed to the handler, False to reject it.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## interface `ResponseFilter`

Interface for filtering outgoing HTTP responses after the route handler has executed. Implementations can inspect or modify the response during post-processing.

### Methods

#### `function filterResponse( self, HttpRequest req, HttpResponse res ) -> Void`

Filter an outgoing HTTP response after the route handler has executed.

**Parameters**:

- `req` (`HttpRequest`)
- `The original HTTP request.`
- `res` (`HttpResponse`)
- `The HTTP response to filter or modify.`

