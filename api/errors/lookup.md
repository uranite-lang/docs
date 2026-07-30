# uranite.errors.lookup

## Table of Contents

- [Imports](#imports)
- [class `LookupError`](#class-lookuperror)
  - [`LookupError()`](#LookupError)
- [class `IndexError`](#class-indexerror)
  - [`IndexError()`](#IndexError)
- [class `KeyError`](#class-keyerror)
  - [`KeyError()`](#KeyError)

## Imports

- `uranite.convert.convert`
  - `intToString`
- `uranite.errors.error`
  - `Error`

## class `LookupError`

**Extends**: `Error`

Base class for all failed data retrieval operations.

Raised when an element, key, or index cannot be found in a data structure. Subclasses distinguish between positional access failures (IndexError) and key-based access failures (KeyError).

**Parameters**:

- `message` (`String`)
- `Human-readable description of the lookup failure.`
- `code` (`I64`)
- `Numeric error code` (`0 for generic lookup failures`)
- `cause` (`?Error`)
- `Optional underlying error that triggered the lookup failure,`
- `or None if this is the root cause.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function LookupError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a LookupError with a descriptive message.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the lookup failure.`
- `code` (`I64`)
- `Numeric error code identifying the failure category.`
- `cause` (`?Error`)
- `Optional chained error that caused this failure, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `IndexError`

**Extends**: `LookupError` → `Error`

Raised when a positional index is outside the valid range of a sequential container such as ArrayList or StringBuilder.

Stores the requested index and container size for diagnostic purposes. The error message is automatically formatted from these values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Fields

| Name | Type | Access |
|------|------|--------|
| `requestedIndex` | `I64` | public |
| `containerSize` | `I64` | public |

### Methods

#### `function IndexError( self, I64 requestedIndex, I64 containerSize ) -> Void`

Construct an IndexError from the offending index and container size.

The error message is automatically formatted as: "index N out of range for container of size M"

**Parameters**:

- `requestedIndex` (`I64`)
- `The index that was attempted.`
- `containerSize` (`I64`)
- `The current number of elements in the container.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `KeyError`

**Extends**: `LookupError` → `Error`

Raised when a key is not found in an associative container such as HashMap or Kwargs.

Stores the requested key string for diagnostic purposes.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Fields

| Name | Type | Access |
|------|------|--------|
| `requestedKey` | `String` | public |

### Methods

#### `function KeyError( self, String requestedKey ) -> Void`

Construct a KeyError from the missing key.

The error message is automatically formatted as: "key not found: '<key>'"

**Parameters**:

- `requestedKey` (`String`)
- `The key that was looked up but does not exist.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

