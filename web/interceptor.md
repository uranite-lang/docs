# uranite.web.interceptor

## Table of Contents

- [Imports](#imports)
- [interface `HandlerInterceptor`](#interface-handlerinterceptor)
  - [`preHandle()`](#preHandle)
  - [`postHandle()`](#postHandle)
  - [`afterCompletion()`](#afterCompletion)

## Imports

- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## interface `HandlerInterceptor`

Interface for intercepting request handling at three lifecycle points: before the handler executes (preHandle), after the handler executes (postHandle), and after the response is fully complete (afterCompletion). Modeled after Spring's HandlerInterceptor pattern.

### Methods

#### `function preHandle( self, HttpRequest req, HttpResponse res ) -> Boolean`

Called before the route handler executes. Can short-circuit request processing by returning False.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request.`
- `res` (`HttpResponse`)
- `The HTTP response that can be populated to short-circuit handling.`

**Returns**: — True to continue processing to the handler, False to stop.

#### `function postHandle( self, HttpRequest req, HttpResponse res ) -> Void`

Called after the route handler has executed successfully but before the response is sent to the client.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request.`
- `res` (`HttpResponse`)
- `The HTTP response produced by the handler.`

#### `function afterCompletion( self, HttpRequest req, HttpResponse res ) -> Void`

Called after the complete request-response cycle is finished, regardless of whether the handler succeeded or threw an exception. Suitable for resource cleanup.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request.`
- `res` (`HttpResponse`)
- `The HTTP response that was sent to the client.`

