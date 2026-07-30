# uranite.testing.assertions

## Table of Contents

- [Imports](#imports)
- [function `assertTrue`](#function-asserttrue)
  - [`assertTrue()`](#assertTrue)
- [function `assertFalse`](#function-assertfalse)
  - [`assertFalse()`](#assertFalse)
- [function `assertEqualI64`](#function-assertequali64)
  - [`assertEqualI64()`](#assertEqualI64)
- [function `assertNotEqualI64`](#function-assertnotequali64)
  - [`assertNotEqualI64()`](#assertNotEqualI64)
- [function `assertEqualF64`](#function-assertequalf64)
  - [`assertEqualF64()`](#assertEqualF64)
- [function `assertEqualStr`](#function-assertequalstr)
  - [`assertEqualStr()`](#assertEqualStr)
- [function `assertNotEqualStr`](#function-assertnotequalstr)
  - [`assertNotEqualStr()`](#assertNotEqualStr)
- [function `assertGreaterThan`](#function-assertgreaterthan)
  - [`assertGreaterThan()`](#assertGreaterThan)
- [function `assertLessThan`](#function-assertlessthan)
  - [`assertLessThan()`](#assertLessThan)
- [function `assertGreaterOrEqual`](#function-assertgreaterorequal)
  - [`assertGreaterOrEqual()`](#assertGreaterOrEqual)
- [function `assertLessOrEqual`](#function-assertlessorequal)
  - [`assertLessOrEqual()`](#assertLessOrEqual)
- [function `assertBetween`](#function-assertbetween)
  - [`assertBetween()`](#assertBetween)
- [function `fail`](#function-fail)
  - [`fail()`](#fail)
- [function `assertEqualBool`](#function-assertequalbool)
  - [`assertEqualBool()`](#assertEqualBool)
- [function `assertContains`](#function-assertcontains)
  - [`assertContains()`](#assertContains)
- [function `assertStartsWith`](#function-assertstartswith)
  - [`assertStartsWith()`](#assertStartsWith)
- [function `assertEndsWith`](#function-assertendswith)
  - [`assertEndsWith()`](#assertEndsWith)
- [function `assertNone`](#function-assertnone)
  - [`assertNone()`](#assertNone)
- [function `assertNotNone`](#function-assertnotnone)
  - [`assertNotNone()`](#assertNotNone)

## Imports

- `uranite.convert.convert`
  - `boolToString`
  - `floatToString`
  - `intToString`
- `uranite.functions.builtin`
  - `endsWith`
  - `indexOf`
  - `startsWith`
- `uranite.testing.errors`
  - `AssertionError`

## function `assertTrue`

Assert that a condition is True. Raises AssertionError if the condition is False.

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

### Methods

#### `function assertTrue( Boolean condition, String message ) -> Void`

Assert that a condition is True. Raises AssertionError if the condition is False.

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

## function `assertFalse`

Assert that a condition is False. Raises AssertionError if the condition is True.

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

### Methods

#### `function assertFalse( Boolean condition, String message ) -> Void`

Assert that a condition is False. Raises AssertionError if the condition is True.

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

## function `assertEqualI64`

Assert that two I64 values are equal. Raises AssertionError if the values differ, including both values in the error message.

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

### Methods

#### `function assertEqualI64( I64 expected, I64 actual, String message ) -> Void`

Assert that two I64 values are equal. Raises AssertionError if the values differ, including both values in the error message.

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

## function `assertNotEqualI64`

Assert that two I64 values are not equal. Raises AssertionError if the values are the same.

**Parameters**:

- `left` (`I64`)
- `The first integer value.`
- `right` (`I64`)
- `The second integer value expected to differ from the first.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left equals right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function assertNotEqualI64( I64 left, I64 right, String message ) -> Void`

Assert that two I64 values are not equal. Raises AssertionError if the values are the same.

**Parameters**:

- `left` (`I64`)
- `The first integer value.`
- `right` (`I64`)
- `The second integer value expected to differ from the first.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left equals right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `assertEqualF64`

Assert that two F64 values are equal within a tolerance. Raises AssertionError if the absolute difference exceeds epsilon.

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

### Methods

#### `function assertEqualF64( F64 expected, F64 actual, F64 epsilon, String message ) -> Void`

Assert that two F64 values are equal within a tolerance. Raises AssertionError if the absolute difference exceeds epsilon.

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

## function `assertEqualStr`

Assert that two strings are equal. Raises AssertionError if the strings differ, including both values in the error message.

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

### Methods

#### `function assertEqualStr( String expected, String actual, String message ) -> Void`

Assert that two strings are equal. Raises AssertionError if the strings differ, including both values in the error message.

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

## function `assertNotEqualStr`

Assert that two strings are not equal. Raises AssertionError if the strings are identical.

**Parameters**:

- `left` (`String`)
- `The first string value.`
- `right` (`String`)
- `The second string value expected to differ from the first.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left equals right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function assertNotEqualStr( String left, String right, String message ) -> Void`

Assert that two strings are not equal. Raises AssertionError if the strings are identical.

**Parameters**:

- `left` (`String`)
- `The first string value.`
- `right` (`String`)
- `The second string value expected to differ from the first.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left equals right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `assertGreaterThan`

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

### Methods

#### `function assertGreaterThan( I64 left, I64 right, String message ) -> Void`

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

## function `assertLessThan`

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

### Methods

#### `function assertLessThan( I64 left, I64 right, String message ) -> Void`

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

## function `assertGreaterOrEqual`

Assert that the first I64 value is greater than or equal to the second.

**Parameters**:

- `left` (`I64`)
- `The value expected to be greater than or equal to right.`
- `right` (`I64`)
- `The lower bound value.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left is strictly less than right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function assertGreaterOrEqual( I64 left, I64 right, String message ) -> Void`

Assert that the first I64 value is greater than or equal to the second.

**Parameters**:

- `left` (`I64`)
- `The value expected to be greater than or equal to right.`
- `right` (`I64`)
- `The lower bound value.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left is strictly less than right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `assertLessOrEqual`

Assert that the first I64 value is less than or equal to the second.

**Parameters**:

- `left` (`I64`)
- `The value expected to be less than or equal to right.`
- `right` (`I64`)
- `The upper bound value.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left is strictly greater than right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function assertLessOrEqual( I64 left, I64 right, String message ) -> Void`

Assert that the first I64 value is less than or equal to the second.

**Parameters**:

- `left` (`I64`)
- `The value expected to be less than or equal to right.`
- `right` (`I64`)
- `The upper bound value.`
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If left is strictly greater than right.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `assertBetween`

Assert that a value falls within an inclusive range [lower, upper].

**Parameters**:

- `value` (`I64`)
- `The value to check.`
- `lower` (`I64`)
- `The lower bound of the acceptable range` (`inclusive`)
- `upper` (`I64`)
- `The upper bound of the acceptable range` (`inclusive`)
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If value is less than lower or greater than upper.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function assertBetween( I64 value, I64 lower, I64 upper, String message ) -> Void`

Assert that a value falls within an inclusive range [lower, upper].

**Parameters**:

- `value` (`I64`)
- `The value to check.`
- `lower` (`I64`)
- `The lower bound of the acceptable range` (`inclusive`)
- `upper` (`I64`)
- `The upper bound of the acceptable range` (`inclusive`)
- `message` (`String`)
- `A descriptive message included in the error on failure.`

**Raises**:

- `AssertionError` → `Error` — If value is less than lower or greater than upper.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fail`

Unconditionally fail the current test with the given message. Use this to mark code paths that should never be reached.

**Parameters**:

- `message` (`String`)
- `A descriptive message explaining why the test failed.`

**Raises**:

- `AssertionError` → `Error` — Always raised.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fail( String message ) -> Void`

Unconditionally fail the current test with the given message. Use this to mark code paths that should never be reached.

**Parameters**:

- `message` (`String`)
- `A descriptive message explaining why the test failed.`

**Raises**:

- `AssertionError` → `Error` — Always raised.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `assertEqualBool`

Assert that two boolean values are equal.

**Parameters**:

- `expected` (`Boolean`)
- `The expected boolean value.`
- `actual` (`Boolean`)
- `The actual boolean value to check.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If expected != actual.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function assertEqualBool( Boolean expected, Boolean actual, String message ) -> Void`

Assert that two boolean values are equal.

**Parameters**:

- `expected` (`Boolean`)
- `The expected boolean value.`
- `actual` (`Boolean`)
- `The actual boolean value to check.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If expected != actual.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `assertContains`

Assert that haystack contains the needle substring.

**Parameters**:

- `haystack` (`String`)
- `The string to search within.`
- `needle` (`String`)
- `The substring to search for.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If needle is not found in haystack.

**Complexity**:
- Time: `O(n * m) where n = haystack length, m = needle length`

### Methods

#### `function assertContains( String haystack, String needle, String message ) -> Void`

Assert that haystack contains the needle substring.

**Parameters**:

- `haystack` (`String`)
- `The string to search within.`
- `needle` (`String`)
- `The substring to search for.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If needle is not found in haystack.

**Complexity**:
- Time: `O(n * m) where n = haystack length, m = needle length`

## function `assertStartsWith`

Assert that source starts with the given prefix.

**Parameters**:

- `source` (`String`)
- `The string to check.`
- `prefix` (`String`)
- `The expected prefix.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If source does not start with prefix.

**Complexity**:
- Time: `O(m) where m = prefix length`

### Methods

#### `function assertStartsWith( String source, String prefix, String message ) -> Void`

Assert that source starts with the given prefix.

**Parameters**:

- `source` (`String`)
- `The string to check.`
- `prefix` (`String`)
- `The expected prefix.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If source does not start with prefix.

**Complexity**:
- Time: `O(m) where m = prefix length`

## function `assertEndsWith`

Assert that source ends with the given suffix.

**Parameters**:

- `source` (`String`)
- `The string to check.`
- `suffix` (`String`)
- `The expected suffix.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If source does not end with suffix.

**Complexity**:
- Time: `O(m) where m = suffix length`

### Methods

#### `function assertEndsWith( String source, String suffix, String message ) -> Void`

Assert that source ends with the given suffix.

**Parameters**:

- `source` (`String`)
- `The string to check.`
- `suffix` (`String`)
- `The expected suffix.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If source does not end with suffix.

**Complexity**:
- Time: `O(m) where m = suffix length`

## function `assertNone`

Assert that a value is None.

**Parameters**:

- `value` (`Object`)
- `The value to check.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If value is not None.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function assertNone( Object value, String message ) -> Void`

Assert that a value is None.

**Parameters**:

- `value` (`Object`)
- `The value to check.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If value is not None.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `assertNotNone`

Assert that a value is not None.

**Parameters**:

- `value` (`Object`)
- `The value to check.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If value is None.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function assertNotNone( Object value, String message ) -> Void`

Assert that a value is not None.

**Parameters**:

- `value` (`Object`)
- `The value to check.`
- `message` (`String`)
- `Descriptive label for the assertion.`

**Raises**:

- `AssertionError` → `Error` — If value is None.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

