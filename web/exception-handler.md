# uranite.web.exception-handler

## Table of Contents

- [Imports](#imports)
- [class `ExceptionHandler`](#class-exceptionhandler)
  - [`ExceptionHandler()`](#ExceptionHandler)
  - [`handleDefault()`](#handleDefault)

## Imports

- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## class `ExceptionHandler`

Base class for exception handlers that convert unhandled errors into HTTP responses. Subclasses can override handleDefault or add specialized handler methods for different error types. Similar to Spring's ControllerAdvice pattern but uses explicit method calls.

### Methods

#### `function ExceptionHandler( self ) -> Void`

Create a new ExceptionHandler with no registered error mappings.

#### `function handleDefault( self, HttpRequest req, HttpResponse res, String errorMessage ) -> Void`

Handle an unhandled exception by generating a 500 Internal Server Error JSON response containing the error message.

**Parameters**:

- `req` (`HttpRequest`)
- `The HTTP request that caused the exception.`
- `res` (`HttpResponse`)
- `The HTTP response to populate with the error details.`
- `errorMessage` (`String`)
- `The error message from the caught exception.`

