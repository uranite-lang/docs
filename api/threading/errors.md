# uranite.threading.errors

## Table of Contents

- [Imports](#imports)
- [class `ThreadError`](#class-threaderror)
  - [`ThreadError()`](#ThreadError)
- [class `ThreadPoisonError`](#class-threadpoisonerror)
  - [`ThreadPoisonError()`](#ThreadPoisonError)
- [class `ThreadChannelError`](#class-threadchannelerror)
  - [`ThreadChannelError()`](#ThreadChannelError)
- [class `ThreadCancellationError`](#class-threadcancellationerror)
  - [`ThreadCancellationError()`](#ThreadCancellationError)
- [class `ThreadTimeoutError`](#class-threadtimeouterror)
  - [`ThreadTimeoutError()`](#ThreadTimeoutError)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.errors.throwable`
  - `Throwable`

## class `ThreadError`

**Extends**: `Error`

Base error type for all threading-related failures. Covers thread lifecycle errors, synchronization problems, and resource exhaustion within the threading module.

### Methods

#### `function ThreadError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a thread error with diagnostic information.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the error.`
- `code` (`I64`)
- `Numeric error code for programmatic identification.`
- `previous` (`?Throwable`)
- `Optional cause that triggered this error.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ThreadPoisonError`

**Extends**: `ThreadError` → `Error`

Error raised when attempting to acquire a mutex or lock that has been poisoned due to a thread panicking while holding it.

### Methods

#### `function ThreadPoisonError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a poison error with diagnostic information.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the poisoning.`
- `code` (`I64`)
- `Numeric error code for programmatic identification.`
- `previous` (`?Throwable`)
- `Optional cause that triggered the poisoning.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ThreadChannelError`

**Extends**: `ThreadError` → `Error`

Error raised when a channel operation fails, such as sending on a closed channel or receiving from a closed and empty channel.

### Methods

#### `function ThreadChannelError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a channel error with diagnostic information.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the channel failure.`
- `code` (`I64`)
- `Numeric error code for programmatic identification.`
- `previous` (`?Throwable`)
- `Optional cause that triggered the error.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ThreadCancellationError`

**Extends**: `ThreadError` → `Error`

Error raised when an operation is cancelled, such as awaiting a future that was cancelled before completion.

### Methods

#### `function ThreadCancellationError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a cancellation error with diagnostic information.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the cancellation.`
- `code` (`I64`)
- `Numeric error code for programmatic identification.`
- `previous` (`?Throwable`)
- `Optional cause that triggered the cancellation.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ThreadTimeoutError`

**Extends**: `ThreadError` → `Error`

Error raised when an operation exceeds its allowed time limit, such as a timed lock acquisition or a timed channel receive.

### Methods

#### `function ThreadTimeoutError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a timeout error with diagnostic information.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the timeout.`
- `code` (`I64`)
- `Numeric error code for programmatic identification.`
- `previous` (`?Throwable`)
- `Optional cause that triggered the timeout.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

