# uranite.language.byte

## Table of Contents

- [class `Byte`](#class-byte)
  - [`Byte()`](#Byte)
  - [`getValue()`](#getValue)
  - [`toString()`](#toString)
  - [`toInt()`](#toInt)
  - [`bitwiseAnd()`](#bitwiseAnd)
  - [`bitwiseOr()`](#bitwiseOr)
  - [`bitwiseXor()`](#bitwiseXor)
  - [`shiftLeft()`](#shiftLeft)
  - [`shiftRight()`](#shiftRight)

## class `Byte`

Unsigned 8-bit value (0-255) with bitwise and shift operations. Wraps a U8 value and provides methods for bitwise AND, OR, XOR, and bit shifting.

### Fields

| Name | Type | Access |
|------|------|--------|
| `value` | `U8` | protect |

### Methods

#### `function Byte( self, U8 value ) -> Void`

Construct a new Byte wrapping the given U8 value. 

#### `function getValue( self ) -> U8`

Return the underlying U8 value. 

#### `function toString( self ) -> String`

Return the string representation of this Byte's numeric value. 

#### `function toInt( self ) -> I32`

Convert this Byte to a signed 32-bit integer. 

#### `function bitwiseAnd( self, Byte other ) -> Byte`

Return a new Byte with the bitwise AND of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function bitwiseOr( self, Byte other ) -> Byte`

Return a new Byte with the bitwise OR of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function bitwiseXor( self, Byte other ) -> Byte`

Return a new Byte with the bitwise XOR of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function shiftLeft( self, I32 amount ) -> Byte`

Return a new Byte with the bits shifted left by the given amount.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function shiftRight( self, I32 amount ) -> Byte`

Return a new Byte with the bits shifted right by the given amount.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

