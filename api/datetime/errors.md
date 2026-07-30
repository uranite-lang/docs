# uranite.datetime.errors

## Table of Contents

- [Imports](#imports)
- [class `DateTimeError`](#class-datetimeerror)
  - [`DateTimeError()`](#DateTimeError)
- [class `DateParseError`](#class-dateparseerror)
  - [`DateParseError()`](#DateParseError)
- [class `InvalidDateError`](#class-invaliddateerror)
  - [`InvalidDateError()`](#InvalidDateError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `DateTimeError`

**Extends**: `Error`

Base error class for all date and time related failures such as invalid date components, timezone lookup failures, or arithmetic overflow in temporal calculations.

### Methods

#### `function DateTimeError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new DateTimeError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the error.`
- `code` (`I64`)
- `A numeric error code identifying the failure type.`
- `cause` (`?Error`)
- `An optional underlying error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `DateParseError`

**Extends**: `DateTimeError` → `Error`

Raised when a date or time string cannot be parsed into a valid DateTime due to malformed input or an unrecognized format.

### Methods

#### `function DateParseError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new DateParseError.

**Parameters**:

- `message` (`String`)
- `A description of the parsing failure.`
- `code` (`I64`)
- `A numeric error code.`
- `cause` (`?Error`)
- `An optional underlying error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `InvalidDateError`

**Extends**: `DateTimeError` → `Error`

Raised when date components form an invalid date, such as month 13, day 32, or February 30.

### Methods

#### `function InvalidDateError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new InvalidDateError.

**Parameters**:

- `message` (`String`)
- `A description of which date component is invalid.`
- `code` (`I64`)
- `A numeric error code.`
- `cause` (`?Error`)
- `An optional underlying error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

