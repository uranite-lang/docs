# uranite.errors.throwable

## Table of Contents

- [Imports](#imports)
- [interface `Throwable`](#interface-throwable)
  - [`drop()`](#drop)
  - [`getCode()`](#getCode)
  - [`getFile()`](#getFile)
  - [`getLine()`](#getLine)
  - [`getMessage()`](#getMessage)
  - [`getPrevious()`](#getPrevious)
  - [`toString()`](#toString)

## Imports

- `uranite.memory.droper`
  - `Droper`

## interface `Throwable`

**Implements**: `Droper`

Root interface for all error and exception types in Uranite.

Any type that can be used with the raise statement must implement this interface. It defines the common contract for accessing error metadata including the error code, source location, message, and optional chained cause. Implementing types also inherit the Droper contract so that heap resources captured alongside a raised throwable are released when the owning reference leaves scope.

### Methods

#### `function drop( self ) -> Void`

Release all heap resources captured alongside this throwable, such as the traceback recorded at raise time. Called automatically when the owning reference leaves scope; calling it manually more than once releases shared resources twice.

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

#### `function toString( self ) -> String`

Return a string representation of this throwable.

**Returns**: `String` — A human-readable string representation, typically the error message.

