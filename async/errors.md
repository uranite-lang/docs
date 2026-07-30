# uranite.async.errors

## Table of Contents

- [Imports](#imports)
- [class `AsyncRuntimeError`](#class-asyncruntimeerror)
  - [`AsyncRuntimeError()`](#AsyncRuntimeError)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.errors.throwable`
  - `Throwable`

## class `AsyncRuntimeError`

**Extends**: `Error`

Error raised when an async runtime operation fails. Wraps low-level syscall failures from epoll, timerfd, mmap, and scheduler operations into a catchable error with a descriptive message.

### Methods

#### `function AsyncRuntimeError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct an AsyncRuntimeError with the given diagnostic information.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the failure.`
- `code` (`I64`)
- `Numeric error code for programmatic identification.`
- `previous` (`?Throwable`)
- `Optional cause that triggered this error, for chained diagnostics.`

