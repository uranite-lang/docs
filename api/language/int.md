# uranite.language.int

## Table of Contents

- [class `Int`](#class-int)
  - [`Int()`](#Int)
  - [`getValue()`](#getValue)
  - [`toString()`](#toString)
  - [`abs()`](#abs)
  - [`negate()`](#negate)
  - [`add()`](#add)
  - [`subtract()`](#subtract)
  - [`multiply()`](#multiply)
  - [`divide()`](#divide)
  - [`modulo()`](#modulo)
  - [`equals()`](#equals)
  - [`compareTo()`](#compareTo)
  - [`min()`](#min)
  - [`max()`](#max)

## class `Int`

Base class for all signed integer types (I8, I16, I32, I64). Stores the value internally as I64 and provides arithmetic, comparison, and utility methods.

### Fields

| Name | Type | Access |
|------|------|--------|
| `value` | `I64` | protect |

### Methods

#### `function Int( self, I64 value ) -> Void`

Construct a new Int wrapping the given I64 value. 

#### `function getValue( self ) -> I64`

Return the underlying I64 value. 

#### `function toString( self ) -> String`

Return the string representation of this integer value. 

#### `function abs( self ) -> Int`

Return a new Int containing the absolute value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function negate( self ) -> Int`

Return a new Int with the sign inverted.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function add( self, Int other ) -> Int`

Return a new Int containing the sum of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function subtract( self, Int other ) -> Int`

Return a new Int containing the difference of self minus other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function multiply( self, Int other ) -> Int`

Return a new Int containing the product of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function divide( self, Int other ) -> Int`

Return a new Int containing the quotient of self divided by other using integer division.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function modulo( self, Int other ) -> Int`

Return a new Int containing the remainder of self divided by other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function equals( self, Int other ) -> Boolean`

Return True if self and other have the same numeric value. 

#### `function compareTo( self, Int other ) -> I32`

Compare self to other, returning -1 if less, 0 if equal, or 1 if greater.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function min( self, Int other ) -> Int`

Return a new Int containing the smaller of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function max( self, Int other ) -> Int`

Return a new Int containing the larger of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

