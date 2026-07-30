# uranite.language.uint

## Table of Contents

- [class `UInt`](#class-uint)
  - [`UInt()`](#UInt)
  - [`getValue()`](#getValue)
  - [`toString()`](#toString)
  - [`add()`](#add)
  - [`subtract()`](#subtract)
  - [`multiply()`](#multiply)
  - [`divide()`](#divide)
  - [`modulo()`](#modulo)
  - [`equals()`](#equals)
  - [`compareTo()`](#compareTo)
  - [`bitwiseAnd()`](#bitwiseAnd)
  - [`bitwiseOr()`](#bitwiseOr)
  - [`bitwiseXor()`](#bitwiseXor)

## class `UInt`

Base class for all unsigned integer types (U8, U16, U32, U64). Stores the value internally as U64 and provides arithmetic, comparison, and bitwise operations.

### Fields

| Name | Type | Access |
|------|------|--------|
| `value` | `U64` | protect |

### Methods

#### `function UInt( self, U64 value ) -> Void`

Construct a new UInt wrapping the given U64 value. 

#### `function getValue( self ) -> U64`

Return the underlying U64 value. 

#### `function toString( self ) -> String`

Return the string representation of this unsigned integer value. 

#### `function add( self, UInt other ) -> UInt`

Return a new UInt containing the sum of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function subtract( self, UInt other ) -> UInt`

Return a new UInt containing the difference of self minus other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function multiply( self, UInt other ) -> UInt`

Return a new UInt containing the product of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function divide( self, UInt other ) -> UInt`

Return a new UInt containing the quotient of self divided by other using integer division.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function modulo( self, UInt other ) -> UInt`

Return a new UInt containing the remainder of self divided by other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function equals( self, UInt other ) -> Boolean`

Return True if self and other have the same numeric value. 

#### `function compareTo( self, UInt other ) -> I32`

Compare self to other, returning -1 if less, 0 if equal, or 1 if greater.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function bitwiseAnd( self, UInt other ) -> UInt`

Return a new UInt with the bitwise AND of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function bitwiseOr( self, UInt other ) -> UInt`

Return a new UInt with the bitwise OR of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function bitwiseXor( self, UInt other ) -> UInt`

Return a new UInt with the bitwise XOR of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

