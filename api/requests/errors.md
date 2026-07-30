# uranite.requests.errors

## Table of Contents

- [Imports](#imports)
- [class `RequestError`](#class-requesterror)
  - [`RequestError()`](#RequestError)
- [class `RequestConnectionError`](#class-requestconnectionerror)
  - [`RequestConnectionError()`](#RequestConnectionError)
- [class `RequestTimeoutError`](#class-requesttimeouterror)
  - [`RequestTimeoutError()`](#RequestTimeoutError)
- [class `HTTPError`](#class-httperror)
  - [`HTTPError()`](#HTTPError)
- [class `InvalidURLError`](#class-invalidurlerror)
  - [`InvalidURLError()`](#InvalidURLError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `RequestError`

**Extends**: `Error`

Base error class for all HTTP request failures.

Extends Error to provide a common ancestor for connection errors, timeout errors, and HTTP status errors raised by the requests library.

### Methods

#### `function RequestError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a new RequestError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the error.`
- `code` (`I64`)
- `An error code identifying the failure type.`
- `cause` (`?Error`)
- `An optional underlying error that caused this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `RequestConnectionError`

**Extends**: `RequestError` → `Error`

Error raised when a connection to the remote server cannot be established.

Indicates a network-level failure such as a refused connection, DNS resolution failure, or unreachable host.

### Methods

#### `function RequestConnectionError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a new RequestConnectionError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the connection failure.`
- `code` (`I64`)
- `An error code identifying the failure type.`
- `cause` (`?Error`)
- `An optional underlying error that caused this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `RequestTimeoutError`

**Extends**: `RequestError` → `Error`

Error raised when an HTTP request exceeds its allowed time limit.

Indicates that the server did not respond within the expected timeout window.

### Methods

#### `function RequestTimeoutError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a new RequestTimeoutError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the timeout.`
- `code` (`I64`)
- `An error code identifying the failure type.`
- `cause` (`?Error`)
- `An optional underlying error that caused this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `HTTPError`

**Extends**: `RequestError` → `Error`

Error raised when an HTTP response indicates a non-success status code.

Carries the HTTP status code from the response, allowing callers to distinguish between different HTTP error categories (4xx client errors, 5xx server errors).

### Fields

| Name | Type | Access |
|------|------|--------|
| `statusCode` | `I64` | public |

### Methods

#### `function HTTPError( self, String message, I64 statusCode, ?Error cause ) -> Void`

Construct a new HTTPError with the given status code.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the HTTP error.`
- `statusCode` (`I64`)
- `The HTTP status code from the response.`
- `cause` (`?Error`)
- `An optional underlying error that caused this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `InvalidURLError`

**Extends**: `RequestError` → `Error`

Error raised when a URL string cannot be parsed into valid components.

### Methods

#### `function InvalidURLError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a new InvalidURLError.

**Parameters**:

- `message` (`String`)
- `Description of the URL parsing failure.`
- `code` (`I64`)
- `Error code.`
- `cause` (`?Error`)
- `Optional underlying error.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

