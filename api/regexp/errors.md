# uranite.regexp.errors

## Table of Contents

- [Imports](#imports)
- [class `RegexSyntaxError`](#class-regexsyntaxerror)
  - [`RegexSyntaxError()`](#RegexSyntaxError)
- [class `RegexRuntimeError`](#class-regexruntimeerror)
  - [`RegexRuntimeError()`](#RegexRuntimeError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `RegexSyntaxError`

**Extends**: `Error`

Raised when a regular expression pattern contains invalid syntax such as unmatched brackets, trailing backslashes, or malformed quantifiers.

### Methods

#### `function RegexSyntaxError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a regex syntax error.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the syntax problem.`
- `code` (`I64`)
- `Numeric error code for programmatic handling.`
- `cause` (`?Error`)
- `Optional underlying error that caused this one.`

## class `RegexRuntimeError`

**Extends**: `Error`

Raised when a regex operation fails at runtime due to excessive backtracking, stack overflow, or other execution limits.

### Methods

#### `function RegexRuntimeError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a regex runtime error.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the runtime failure.`
- `code` (`I64`)
- `Numeric error code for programmatic handling.`
- `cause` (`?Error`)
- `Optional underlying error that caused this one.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

