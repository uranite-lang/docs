# uranite.net.http.errors

## Table of Contents

- [Imports](#imports)
- [class `HttpError`](#class-httperror)
  - [`HttpError()`](#HttpError)
- [class `HttpTimeoutError`](#class-httptimeouterror)
  - [`HttpTimeoutError()`](#HttpTimeoutError)
- [class `HttpConnectionError`](#class-httpconnectionerror)
  - [`HttpConnectionError()`](#HttpConnectionError)
- [class `HttpParseError`](#class-httpparseerror)
  - [`HttpParseError()`](#HttpParseError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `HttpError`

**Extends**: `Error`

Base error for all HTTP operations including connection, timeout, and parsing failures.

### Methods

#### `function HttpError( self, String message, I64 code, ?Error cause ) -> Void`

Construct an HTTP error.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the error.`
- `code` (`I64`)
- `Numeric error code for programmatic handling.`
- `cause` (`?Error`)
- `Optional underlying error that caused this one.`

## class `HttpTimeoutError`

**Extends**: `HttpError` → `Error`

Raised when an HTTP operation exceeds its time limit.

### Methods

#### `function HttpTimeoutError( self, String message, I64 code, ?Error cause ) -> Void`

Construct an HTTP timeout error.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the timeout.`
- `code` (`I64`)
- `Numeric error code for programmatic handling.`
- `cause` (`?Error`)
- `Optional underlying error that caused this one.`

## class `HttpConnectionError`

**Extends**: `HttpError` → `Error`

Raised when a TCP connection to the remote host cannot be established.

### Methods

#### `function HttpConnectionError( self, String message, I64 code, ?Error cause ) -> Void`

Construct an HTTP connection error.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the connection failure.`
- `code` (`I64`)
- `Numeric error code, typically the negated errno.`
- `cause` (`?Error`)
- `Optional underlying error that caused this one.`

## class `HttpParseError`

**Extends**: `HttpError` → `Error`

Raised when an HTTP response cannot be parsed due to malformed status line, headers, or body.

### Methods

#### `function HttpParseError( self, String message, I64 code, ?Error cause ) -> Void`

Construct an HTTP parse error.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the parse failure.`
- `code` (`I64`)
- `Numeric error code for programmatic handling.`
- `cause` (`?Error`)
- `Optional underlying error that caused this one.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

