# uranite.errors.value

## Table of Contents

- [Imports](#imports)
- [class `ValueError`](#class-valueerror)
  - [`ValueError()`](#ValueError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `ValueError`

**Extends**: `Error`

Raised when an operation receives an argument of the correct type but with an inappropriate or invalid value.

Examples: negative array size, empty string where non-empty required, chunk size of zero, exponent below zero for integer power.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function ValueError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a ValueError with a descriptive message.

**Parameters**:

- `message` (`String`)
- `Human-readable description of why the value is invalid.`
- `code` (`I64`)
- `Numeric error code` (`0 for generic value errors`)
- `cause` (`?Error`)
- `Optional chained error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

