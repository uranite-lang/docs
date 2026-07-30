# uranite.web.errors

## Table of Contents

- [Imports](#imports)
- [class `HttpError`](#class-httperror)
  - [`HttpError()`](#HttpError)
- [class `BadRequestError`](#class-badrequesterror)
  - [`BadRequestError()`](#BadRequestError)
- [class `UnauthorizedError`](#class-unauthorizederror)
  - [`UnauthorizedError()`](#UnauthorizedError)
- [class `ForbiddenError`](#class-forbiddenerror)
  - [`ForbiddenError()`](#ForbiddenError)
- [class `NotFoundError`](#class-notfounderror)
  - [`NotFoundError()`](#NotFoundError)
- [class `MethodNotAllowedError`](#class-methodnotallowederror)
  - [`MethodNotAllowedError()`](#MethodNotAllowedError)
- [class `ConflictError`](#class-conflicterror)
  - [`ConflictError()`](#ConflictError)
- [class `InternalServerError`](#class-internalservererror)
  - [`InternalServerError()`](#InternalServerError)
- [class `ServiceUnavailableError`](#class-serviceunavailableerror)
  - [`ServiceUnavailableError()`](#ServiceUnavailableError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `HttpError`

**Extends**: `Error`

Base error for all HTTP-related failures. Carries an HTTP status code in addition to the standard error message and cause chain.

### Fields

| Name | Type | Access |
|------|------|--------|
| `statusCode` | `I64` | public |

### Methods

#### `function HttpError( self, String message, I64 statusCode, ?Error cause ) -> Void`

Create a new HttpError with a message, HTTP status code, and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the HTTP error.`
- `statusCode` (`I64`)
- `The HTTP status code` (`e.g., 400, 404, 500`)
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `BadRequestError`

**Extends**: `HttpError` → `Error`

Error representing an HTTP 400 Bad Request response, raised when the client sends a malformed or invalid request.

### Methods

#### `function BadRequestError( self, String message, ?Error cause ) -> Void`

Create a new BadRequestError (HTTP 400) with a message and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of what was wrong with the request.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `UnauthorizedError`

**Extends**: `HttpError` → `Error`

Error representing an HTTP 401 Unauthorized response, raised when authentication is required but was not provided or is invalid.

### Methods

#### `function UnauthorizedError( self, String message, ?Error cause ) -> Void`

Create a new UnauthorizedError (HTTP 401) with a message and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the authentication failure.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ForbiddenError`

**Extends**: `HttpError` → `Error`

Error representing an HTTP 403 Forbidden response, raised when the client is authenticated but lacks permission to access the resource.

### Methods

#### `function ForbiddenError( self, String message, ?Error cause ) -> Void`

Create a new ForbiddenError (HTTP 403) with a message and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the authorization failure.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `NotFoundError`

**Extends**: `HttpError` → `Error`

Error representing an HTTP 404 Not Found response, raised when the requested resource does not exist on the server.

### Methods

#### `function NotFoundError( self, String message, ?Error cause ) -> Void`

Create a new NotFoundError (HTTP 404) with a message and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the missing resource.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `MethodNotAllowedError`

**Extends**: `HttpError` → `Error`

Error representing an HTTP 405 Method Not Allowed response, raised when the request method is not supported for the target resource.

### Methods

#### `function MethodNotAllowedError( self, String message, ?Error cause ) -> Void`

Create a new MethodNotAllowedError (HTTP 405) with a message and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the method restriction.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ConflictError`

**Extends**: `HttpError` → `Error`

Error representing an HTTP 409 Conflict response, raised when the request conflicts with the current state of the resource.

### Methods

#### `function ConflictError( self, String message, ?Error cause ) -> Void`

Create a new ConflictError (HTTP 409) with a message and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the conflict.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `InternalServerError`

**Extends**: `HttpError` → `Error`

Error representing an HTTP 500 Internal Server Error response, raised when the server encounters an unexpected condition that prevents it from fulfilling the request.

### Methods

#### `function InternalServerError( self, String message, ?Error cause ) -> Void`

Create a new InternalServerError (HTTP 500) with a message and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the server error.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ServiceUnavailableError`

**Extends**: `HttpError` → `Error`

Error representing an HTTP 503 Service Unavailable response, raised when the server is temporarily unable to handle the request due to maintenance or overload.

### Methods

#### `function ServiceUnavailableError( self, String message, ?Error cause ) -> Void`

Create a new ServiceUnavailableError (HTTP 503) with a message and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the unavailability.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

