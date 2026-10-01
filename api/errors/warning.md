# uranite.errors.warning

## Table of Contents

- [Imports](#imports)
- [class `Warning`](#class-warning)
  - [`Warning()`](#Warning)
  - [`getCode()`](#getCode)
  - [`getFile()`](#getFile)
  - [`getLine()`](#getLine)
  - [`getMessage()`](#getMessage)
  - [`getPrevious()`](#getPrevious)
  - [`getTraceback()`](#getTraceback)
  - [`drop()`](#drop)
  - [`toString()`](#toString)

## Imports

- `uranite.errors.runtime`
  - `__uranite_release_traceback_of`
- `uranite.errors.throwable`
  - `Throwable`
- `uranite.errors.traceback.traceback`
  - `Traceback`

## class `Warning`

**Implements**: `Throwable`

Non-fatal condition that may require attention but is less severe than an Error.

Warnings represent situations that are not necessarily wrong but could indicate potential problems, such as deprecated API usage or suspicious but valid input. They implement the Throwable interface and can be raised and caught like exceptions.

### Fields

| Name | Type | Access |
|------|------|--------|
| `code` | `I64` | protect |
| `file` | `String` | protect |
| `line` | `I64` | protect |
| `message` | `String` | protect |
| `previous` | `?Throwable` | protect |
| `traceback` | `?Traceback` | protect |

### Methods

#### `function Warning( self, String message, I64 code, ?Throwable previous ) -> Void`

Create a new warning with a message, code, and optional chained cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the warning condition.`
- `code` (`I64`)
- `A numeric code identifying the type of warning.`
- `previous` (`?Throwable`)
- `An optional previous throwable that caused this warning, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCode( self ) -> I64`

#### `function getFile( self ) -> String`

#### `function getLine( self ) -> I64`

#### `function getMessage( self ) -> String`

#### `function getPrevious( self ) -> ?Throwable`

#### `function getTraceback( self ) -> ?Traceback`

#### `function drop( self ) -> Void`

Release the heap-allocated traceback captured when this warning was raised, freeing the frame buffer, every captured frame record, and the traceback container. Called automatically when the owning reference leaves scope; calling it manually more than once releases shared memory twice.

**Complexity**:
- Time: `O(f) where f is the number of captured stack frames.`
- Space: `O(1)`

#### `function toString( self ) -> String`

