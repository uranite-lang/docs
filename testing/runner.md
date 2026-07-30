# uranite.testing.runner

## Table of Contents

- [Imports](#imports)
- [class `TestRunner`](#class-testrunner)
  - [`TestRunner()`](#TestRunner)
  - [`recordPass()`](#recordPass)
  - [`recordFail()`](#recordFail)
  - [`recordError()`](#recordError)
  - [`report()`](#report)
  - [`allPassed()`](#allPassed)

## Imports

- `uranite.convert.convert`
  - `intToString`
- `uranite.datetime.datetime`
  - `nowMonotonic`
- `uranite.io.console`
  - `putsln`

## class `TestRunner`

Collects and executes test functions, tracking pass, fail, and error counts. Prints a formatted summary report after all tests have run.

### Fields

| Name | Type | Access |
|------|------|--------|
| `passed` | `I64` | public |
| `failed` | `I64` | public |
| `errors` | `I64` | public |
| `total` | `I64` | public |

### Methods

#### `function TestRunner( self ) -> Void`

Create a new TestRunner with all counters initialized to zero.

#### `function recordPass( self, String name ) -> Void`

Record a passing test result and print its name to the console.

**Parameters**:

- `name` (`String`)
- `The name of the test that passed.`

#### `function recordFail( self, String name, String message ) -> Void`

Record a failing test result and print its name with the failure message to the console.

**Parameters**:

- `name` (`String`)
- `The name of the test that failed.`
- `message` (`String`)
- `A description of why the test failed.`

#### `function recordError( self, String name, String message ) -> Void`

Record a test that encountered an unexpected error and print its name with the error message to the console.

**Parameters**:

- `name` (`String`)
- `The name of the test that errored.`
- `message` (`String`)
- `A description of the unexpected error.`

#### `function report( self ) -> Void`

Print a formatted summary of all test results to the console, including total, passed, failed, and error counts. Concludes with either "ALL TESTS PASSED" or "TESTS FAILED".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function allPassed( self ) -> Boolean`

Check whether all recorded tests passed without any failures or errors.

**Returns**: — True if no tests failed and no tests errored, False otherwise.

