# uranite.language.float

## Table of Contents

- [class `Float`](#class-float)
  - [`Float()`](#Float)
  - [`getValue()`](#getValue)
  - [`toString()`](#toString)
  - [`abs()`](#abs)
  - [`negate()`](#negate)
  - [`add()`](#add)
  - [`subtract()`](#subtract)
  - [`multiply()`](#multiply)
  - [`divide()`](#divide)
  - [`equals()`](#equals)
  - [`compareTo()`](#compareTo)
  - [`isNaN()`](#isNaN)
  - [`isInfinite()`](#isInfinite)
  - [`floor()`](#floor)
  - [`ceil()`](#ceil)
  - [`round()`](#round)
  - [`toInt()`](#toInt)

## class `Float`

Base class for all floating-point types (F32, F64). Stores the value internally as F64 and provides arithmetic, comparison, and numeric utility methods.

### Fields

| Name | Type | Access |
|------|------|--------|
| `value` | `F64` | protect |

### Methods

#### `function Float( self, F64 value ) -> Void`

Construct a new Float wrapping the given F64 value. 

#### `function getValue( self ) -> F64`

Return the underlying F64 value. 

#### `function toString( self ) -> String`

Return the string representation of this floating-point value. 

#### `function abs( self ) -> Float`

Return a new Float containing the absolute value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function negate( self ) -> Float`

Return a new Float with the sign inverted.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function add( self, Float other ) -> Float`

Return a new Float containing the sum of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function subtract( self, Float other ) -> Float`

Return a new Float containing the difference of self minus other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function multiply( self, Float other ) -> Float`

Return a new Float containing the product of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function divide( self, Float other ) -> Float`

Return a new Float containing the quotient of self divided by other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function equals( self, Float other ) -> Boolean`

Return True if self and other have the same numeric value. 

#### `function compareTo( self, Float other ) -> I32`

Compare self to other, returning -1 if less, 0 if equal, or 1 if greater.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isNaN( self ) -> Boolean`

Return True if this value is Not-a-Number, detected by self-inequality. 

#### `function isInfinite( self ) -> Boolean`

Return True if this value is positive or negative infinity. 

#### `function floor( self ) -> Float`

Return a new Float rounded down to the nearest integer value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function ceil( self ) -> Float`

Return a new Float rounded up to the nearest integer value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function round( self ) -> Float`

Return a new Float rounded to the nearest integer value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toInt( self ) -> I64`

Convert this floating-point value to a signed 64-bit integer by truncation. 

