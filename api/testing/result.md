# uranite.testing.result

## Table of Contents

- [enum `TestStatus`](#enum-teststatus)
- [class `TestResult`](#class-testresult)
  - [`TestResult()`](#TestResult)
  - [`isPassed()`](#isPassed)
  - [`isFailed()`](#isFailed)
  - [`isError()`](#isError)
  - [`isSkipped()`](#isSkipped)

## enum `TestStatus`

Status of a completed test case.

Use variant.name for string representation (e.g., TestStatus.Pass.name returns "Pass").

## class `TestResult`

### Fields

| Name | Type | Access |
|------|------|--------|
| `name` | `String` | public |
| `status` | `TestStatus` | public |
| `message` | `String` | public |
| `durationNanos` | `I64` | public |

### Methods

#### `function TestResult( self, String name, TestStatus status, String message, I64 durationNanos ) -> Void`

#### `function isPassed( self ) -> Boolean`

#### `function isFailed( self ) -> Boolean`

#### `function isError( self ) -> Boolean`

#### `function isSkipped( self ) -> Boolean`

