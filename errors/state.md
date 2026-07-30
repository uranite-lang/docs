# uranite.errors.state

## Table of Contents

- [Imports](#imports)
- [class `StateError`](#class-stateerror)
  - [`StateError()`](#StateError)
- [class `CapacityError`](#class-capacityerror)
  - [`CapacityError()`](#CapacityError)
- [class `NotImplementedError`](#class-notimplementederror)
  - [`NotImplementedError()`](#NotImplementedError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `StateError`

**Extends**: `Error`

Raised when an operation is invalid for the current state of an object.

Examples: calling next() on an exhausted iterator, reading from a closed file, sending on a shutdown channel.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function StateError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a StateError with a descriptive message.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the invalid state.`
- `code` (`I64`)
- `Numeric error code` (`0 for generic state errors`)
- `cause` (`?Error`)
- `Optional chained error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `CapacityError`

**Extends**: `StateError` → `Error`

Raised when a container or allocator has reached its maximum capacity and cannot accept additional elements.

Examples: arena allocator exhausted, fixed-size buffer full, thread pool at worker limit.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function CapacityError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a CapacityError with a descriptive message.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the capacity exhaustion.`
- `code` (`I64`)
- `Numeric error code` (`0 for generic capacity errors`)
- `cause` (`?Error`)
- `Optional chained error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `NotImplementedError`

**Extends**: `Error`

Raised when an abstract method stub has not been overridden by a concrete subclass, or when a feature is intentionally left unimplemented as a placeholder.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Fields

| Name | Type | Access |
|------|------|--------|
| `methodName` | `String` | public |

### Methods

#### `function NotImplementedError( self, String methodName ) -> Void`

Construct a NotImplementedError from the unimplemented method name.

The error message is automatically formatted as: "method not implemented: '<methodName>'"

**Parameters**:

- `methodName` (`String`)
- `The name of the method that lacks an implementation.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

