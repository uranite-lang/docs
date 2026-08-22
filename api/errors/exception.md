# uranite.errors.exception

## Table of Contents

- [Imports](#imports)
- [class `Exception`](#class-exception)
  - [`Exception()`](#Exception)
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

## class `Exception`

**Implements**: `Throwable`

Recoverable error condition that applications may catch and handle.

Exceptions represent problems that a well-written application should anticipate and recover from, such as invalid input, file-not-found errors, or network failures. They implement the Throwable interface for use with the raise and catch mechanism.

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

#### `function Exception( self, String message, I64 code, ?Throwable previous ) -> Void`

Create a new exception with a message, error code, and optional chained cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the exception condition.`
- `code` (`I64`)
- `A numeric error code identifying the type of exception.`
- `previous` (`?Throwable`)
- `An optional previous throwable that caused this exception, or None.`

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

