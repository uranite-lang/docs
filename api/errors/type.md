# uranite.errors.type

## Table of Contents

- [Imports](#imports)
- [class `TypeError`](#class-typeerror)
  - [`TypeError()`](#TypeError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `TypeError`

**Extends**: `Error`

Raised when an operation encounters a value of an unexpected type at runtime.

Stores the expected and actual type names for diagnostic messages. The error message is automatically formatted from these values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Fields

| Name | Type | Access |
|------|------|--------|
| `expectedType` | `String` | public |
| `actualType` | `String` | public |

### Methods

#### `function TypeError( self, String expectedType, String actualType ) -> Void`

Construct a TypeError from the expected and actual type names.

The error message is automatically formatted as: "expected type '<expected>' but got '<actual>'"

**Parameters**:

- `expectedType` (`String`)
- `The type name the operation expected.`
- `actualType` (`String`)
- `The type name that was actually received.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

