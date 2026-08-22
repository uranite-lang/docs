# uranite.errors.interrupt

## Table of Contents

- [Imports](#imports)
- [class `KeyboardInterruptError`](#class-keyboardinterrupterror)
  - [`KeyboardInterruptError()`](#KeyboardInterruptError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `KeyboardInterruptError`

**Extends**: `Error`

Raised when the user sends an interrupt signal (SIGINT) to the program, typically by pressing Ctrl+C. Programs can catch this error to perform graceful shutdown or cleanup before exiting.

### Methods

#### `function KeyboardInterruptError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a KeyboardInterruptError with a descriptive message.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the interrupt condition.`
- `code` (`I64`)
- `Numeric error code, typically 130 for SIGINT.`
- `cause` (`?Error`)
- `Optional chained error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

