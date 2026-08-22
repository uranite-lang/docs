# uranite.errors.os

## Table of Contents

- [Imports](#imports)
- [class `OSError`](#class-oserror)
  - [`OSError()`](#OSError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `OSError`

**Extends**: `Error`

Raised when a system-level operation fails due to an operating system error. Wraps errno-style error codes from failed syscalls with a human-readable context message describing the operation that failed.

### Methods

#### `function OSError( self, String message, I64 code, ?Error cause ) -> Void`

Construct an OSError with a descriptive message and errno code.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the failed operation.`
- `code` (`I64`)
- `The errno value from the failed syscall.`
- `cause` (`?Error`)
- `Optional chained error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

