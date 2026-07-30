# uranite.testing.test-case

## Table of Contents

- [Imports](#imports)
- [class `TestCase`](#class-testcase)
  - [`TestCase()`](#TestCase)
  - [`setUp()`](#setUp)
  - [`tearDown()`](#tearDown)
  - [`assertTrue()`](#assertTrue)
  - [`assertFalse()`](#assertFalse)
  - [`assertEqualI64()`](#assertEqualI64)
  - [`assertEqualF64()`](#assertEqualF64)
  - [`assertEqualStr()`](#assertEqualStr)
  - [`assertGreaterThan()`](#assertGreaterThan)
  - [`assertLessThan()`](#assertLessThan)
  - [`fail()`](#fail)

## Imports

- `uranite.convert.convert`
  - `floatToString`
  - `intToString`
- `uranite.testing.errors`
  - `AssertionError`

## class `TestCase`

Base class for unit tests providing lifecycle hooks and built-in assertion methods. Subclasses override setUp and tearDown for per-test initialization and cleanup, and use the assertion methods to verify expected behavior.

### Fields

| Name | Type | Access |
|------|------|--------|
| `testName` | `String` | public |

### Methods

#### `function TestCase( self, String name ) -> Void`

Create a new TestCase with the given name.

**Parameters**:

- `name` (`String`)
- `The name identifying this test case.`

#### `function setUp( self ) -> Void`

Called before each test method runs. Override in subclasses to perform per-test initialization such as creating fixtures or resetting state. The default implementation does nothing.

#### `function tearDown( self ) -> Void`

Called after each test method runs, regardless of whether the test passed or failed. Override in subclasses to perform cleanup such as releasing resources. The default implementation does nothing.

#### `function assertTrue( self, Boolean condition, String message ) -> Void`

Assert that a condition is True.

**Parameters**:

- `condition` (`Boolean`)
- `The boolean value expected to be True.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If condition is False.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function assertFalse( self, Boolean condition, String message ) -> Void`

Assert that a condition is False.

**Parameters**:

- `condition` (`Boolean`)
- `The boolean value expected to be False.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If condition is True.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function assertEqualI64( self, I64 expected, I64 actual, String message ) -> Void`

Assert that two I64 values are equal.

**Parameters**:

- `expected` (`I64`)
- `The expected integer value.`
- `actual` (`I64`)
- `The actual integer value to compare against expected.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If expected does not equal actual.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function assertEqualF64( self, F64 expected, F64 actual, F64 epsilon, String message ) -> Void`

Assert that two F64 values are equal within a tolerance.

**Parameters**:

- `expected` (`F64`)
- `The expected floating-point value.`
- `actual` (`F64`)
- `The actual floating-point value to compare.`
- `epsilon` (`F64`)
- `The maximum allowed absolute difference between expected`
- `and actual for them to be considered equal.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If the absolute difference exceeds epsilon.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function assertEqualStr( self, String expected, String actual, String message ) -> Void`

Assert that two strings are equal.

**Parameters**:

- `expected` (`String`)
- `The expected string value.`
- `actual` (`String`)
- `The actual string value to compare.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If expected does not equal actual.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function assertGreaterThan( self, I64 left, I64 right, String message ) -> Void`

Assert that the first I64 value is strictly greater than the second.

**Parameters**:

- `left` (`I64`)
- `The value expected to be greater.`
- `right` (`I64`)
- `The value expected to be less than left.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left is less than or equal to right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function assertLessThan( self, I64 left, I64 right, String message ) -> Void`

Assert that the first I64 value is strictly less than the second.

**Parameters**:

- `left` (`I64`)
- `The value expected to be less.`
- `right` (`I64`)
- `The value expected to be greater than left.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left is greater than or equal to right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fail( self, String message ) -> Void`

Unconditionally fail the current test with the given message.

**Parameters**:

- `message` (`String`)
- `A descriptive message explaining why the test failed.`

**Raises**:

- `AssertionError` → `Error` — Always raised.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

