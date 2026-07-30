# uranite.process.errors

## Table of Contents

- [Imports](#imports)
- [class `ProcessError`](#class-processerror)
  - [`ProcessError()`](#ProcessError)
- [class `ProcessCreationError`](#class-processcreationerror)
  - [`ProcessCreationError()`](#ProcessCreationError)
- [class `ProcessCommunicationError`](#class-processcommunicationerror)
  - [`ProcessCommunicationError()`](#ProcessCommunicationError)
- [class `ProcessCancellationError`](#class-processcancellationerror)
  - [`ProcessCancellationError()`](#ProcessCancellationError)
- [class `ProcessTimeoutError`](#class-processtimeouterror)
  - [`ProcessTimeoutError()`](#ProcessTimeoutError)
- [class `ProcessCrashedError`](#class-processcrashederror)
  - [`ProcessCrashedError()`](#ProcessCrashedError)
- [class `ProcessPoolError`](#class-processpoolerror)
  - [`ProcessPoolError()`](#ProcessPoolError)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.errors.throwable`
  - `Throwable`

## class `ProcessError`

**Extends**: `Error`

Base error type for all process-related failures.

Serves as the root of the process error hierarchy. All specific process error types extend this class. Can also be raised directly for general process errors that do not fit a more specific category.

### Methods

#### `function ProcessError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new ProcessError with the given diagnostic information.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the error.`
- `code` (`I64`)
- `Numeric error code identifying the failure type.`
- `previous` (`Throwable`)
- `Optional previous throwable that caused this error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ProcessCreationError`

**Extends**: `ProcessError` → `Error`

Raised when a new process cannot be created.

Typical causes include resource exhaustion (stack allocation failure, process ID limit reached) or invalid configuration parameters.

### Methods

#### `function ProcessCreationError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new ProcessCreationError.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the creation failure.`
- `code` (`I64`)
- `Numeric error code identifying the failure type.`
- `previous` (`Throwable`)
- `Optional previous throwable that caused this error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ProcessCommunicationError`

**Extends**: `ProcessError` → `Error`

Raised when inter-process communication fails.

Covers failures in shared memory operations, channel send/receive errors, and pipe I/O failures between processes.

### Methods

#### `function ProcessCommunicationError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new ProcessCommunicationError.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the communication failure.`
- `code` (`I64`)
- `Numeric error code identifying the failure type.`
- `previous` (`Throwable`)
- `Optional previous throwable that caused this error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ProcessCancellationError`

**Extends**: `ProcessError` → `Error`

Raised when a process or process future is cancelled.

Indicates that the operation was intentionally aborted before completion, either by explicit cancellation or by scope teardown.

### Methods

#### `function ProcessCancellationError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new ProcessCancellationError.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the cancellation.`
- `code` (`I64`)
- `Numeric error code identifying the failure type.`
- `previous` (`Throwable`)
- `Optional previous throwable that caused this error, or None.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ProcessTimeoutError`

**Extends**: `ProcessError` → `Error`

Raised when a process operation exceeds its time limit.

Used for timed join operations, timed future waits, and any other process interaction with a deadline that was not met.

### Methods

#### `function ProcessTimeoutError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new ProcessTimeoutError.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the timeout.`
- `code` (`I64`)
- `Numeric error code identifying the failure type.`
- `previous` (`Throwable`)
- `Optional previous throwable that caused this error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ProcessCrashedError`

**Extends**: `ProcessError` → `Error`

Raised when a process terminates abnormally due to an unhandled error.

Indicates that the worker process encountered a fatal condition such as a segmentation fault, unhandled exception, or stack overflow.

### Methods

#### `function ProcessCrashedError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new ProcessCrashedError.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the crash.`
- `code` (`I64`)
- `Numeric error code identifying the failure type.`
- `previous` (`Throwable`)
- `Optional previous throwable that caused this error, or None.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ProcessPoolError`

**Extends**: `ProcessError` → `Error`

Raised for errors specific to process pool operations.

Covers pool-level failures such as invalid worker count configuration, submission to a shut-down pool, or pool resource exhaustion.

### Methods

#### `function ProcessPoolError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new ProcessPoolError.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the pool error.`
- `code` (`I64`)
- `Numeric error code identifying the failure type.`
- `previous` (`Throwable`)
- `Optional previous throwable that caused this error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

