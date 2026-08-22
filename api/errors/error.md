# uranite.errors.error

## Table of Contents

- [Imports](#imports)
- [class `Error`](#class-error)
  - [`Error()`](#Error)
  - [`getCode()`](#getCode)
  - [`getFile()`](#getFile)
  - [`getLine()`](#getLine)
  - [`getMessage()`](#getMessage)
  - [`getPrevious()`](#getPrevious)
  - [`getTraceback()`](#getTraceback)
  - [`toString()`](#toString)

## Imports

- `uranite.errors.throwable`
  - `Throwable`
- `uranite.errors.traceback.traceback`
  - `Traceback`

## class `Error`

**Implements**: `Throwable`

Unrecoverable error that applications should not catch.

Errors represent serious problems that a reasonable application should not attempt to recover from, such as out-of-memory conditions or internal compiler failures. They implement the Throwable interface for use with the raise mechanism.

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

#### `function Error( self, String message, I64 code, ?Throwable previous ) -> Void`

Create a new error with a message, error code, and optional chained cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the error condition.`
- `code` (`I64`)
- `A numeric error code identifying the type of error.`
- `previous` (`?Throwable`)
- `An optional previous throwable that caused this error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCode( self ) -> I64`

#### `function getFile( self ) -> String`

#### `function getLine( self ) -> I64`

#### `function getMessage( self ) -> String`

#### `function getPrevious( self ) -> ?Throwable`

#### `function getTraceback( self ) -> ?Traceback`

#### `function toString( self ) -> String`

