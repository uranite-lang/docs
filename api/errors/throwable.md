# uranite.errors.throwable

## Table of Contents

- [Imports](#imports)
- [interface `Throwable`](#interface-throwable)
  - [`getCode()`](#getCode)
  - [`getFile()`](#getFile)
  - [`getLine()`](#getLine)
  - [`getMessage()`](#getMessage)
  - [`getPrevious()`](#getPrevious)
  - [`getTraceback()`](#getTraceback)
  - [`toString()`](#toString)

## Imports

- `uranite.errors.traceback`
  - `Traceback`

## interface `Throwable`

Root interface for all error and exception types in Uranite.

Any type that can be used with the raise statement must implement this interface. It defines the common contract for accessing error metadata including the error code, source location, message, and optional chained cause.

### Methods

#### `function getCode( self ) -> I64`

Return the numeric error code associated with this throwable.

**Returns**: `I64` — The error code identifying the type of problem.

#### `function getFile( self ) -> String`

Return the source file path where this throwable originated.

**Returns**: `String` — The file path of the source that raised this throwable.

#### `function getLine( self ) -> I64`

Return the source line number where this throwable originated.

**Returns**: `I64` — The line number in the source file.

#### `function getMessage( self ) -> String`

Return the human-readable error message describing the problem.

**Returns**: `String` — A descriptive message explaining the error or exception.

#### `function getPrevious( self ) -> ?Throwable`

Return the chained cause of this throwable, or None if this is the root cause.

**Returns**: `?Throwable` — The previous throwable that caused this one, or None.

#### `function getTraceback( self ) -> ?Traceback`

Return the traceback information for this throwable, or None if not available.

**Returns**: `?Traceback` — The traceback record containing source location info, or None.

#### `function toString( self ) -> String`

Return a string representation of this throwable.

**Returns**: `String` — A human-readable string representation, typically the error message.

