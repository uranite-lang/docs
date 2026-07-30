# uranite.testing.errors

## Table of Contents

- [Imports](#imports)
- [class `AssertionError`](#class-assertionerror)
  - [`AssertionError()`](#AssertionError)
- [class `TestSetupError`](#class-testsetuperror)
  - [`TestSetupError()`](#TestSetupError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `AssertionError`

**Extends**: `Error`

Raised when a test assertion fails, indicating that an expected condition was not met during test execution.

### Methods

#### `function AssertionError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new AssertionError with a descriptive failure message.

**Parameters**:

- `message` (`String`)
- `A description of the assertion that failed.`
- `code` (`I64`)
- `A numeric error code for the assertion failure.`
- `cause` (`?Error`)
- `An optional underlying error that caused the assertion`
- `to fail, or None if there is no root cause.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `TestSetupError`

**Extends**: `Error`

Raised when test setup or teardown fails, preventing the test case from executing properly.

### Methods

#### `function TestSetupError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new TestSetupError indicating a setup or teardown failure.

**Parameters**:

- `message` (`String`)
- `A description of what went wrong during test setup.`
- `code` (`I64`)
- `A numeric error code for the setup failure.`
- `cause` (`?Error`)
- `An optional underlying error that caused the setup`
- `failure, or None if there is no root cause.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

