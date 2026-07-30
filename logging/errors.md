# uranite.logging.errors

## Table of Contents

- [Imports](#imports)
- [class `LoggingError`](#class-loggingerror)
  - [`LoggingError()`](#LoggingError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `LoggingError`

**Extends**: `Error`

Represents an error that occurs during logging operations such as handler failures, formatter errors, or invalid log configuration.

### Methods

#### `function LoggingError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new LoggingError with a descriptive message, error code, and an optional underlying cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the logging error.`
- `code` (`I64`)
- `A numeric error code identifying the type of logging failure.`
- `cause` (`?Error`)
- `An optional underlying error that triggered this logging error,`
- `or None if there is no root cause.`

