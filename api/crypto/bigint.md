# uranite.crypto.bigint

## Table of Contents

- [Imports](#imports)
- [const `LIMB_BITS`](#const-limb-bits)
- [const `LIMB_MASK`](#const-limb-mask)
- [const `LIMB_SIZE`](#const-limb-size)
- [const `MAX_LIMBS`](#const-max-limbs)
- [class `BigInt`](#class-bigint)
  - [`BigInt()`](#BigInt)
  - [`setLimb()`](#setLimb)
  - [`getLimb()`](#getLimb)
  - [`isZero()`](#isZero)
  - [`effectiveLimbs()`](#effectiveLimbs)
  - [`bitLength()`](#bitLength)
  - [`getBit()`](#getBit)
  - [`compare()`](#compare)
  - [`copyFrom()`](#copyFrom)
  - [`destroy()`](#destroy)
- [function `bigintFromBytes`](#function-bigintfrombytes)
  - [`bigintFromBytes()`](#bigintFromBytes)
- [function `bigintToBytes`](#function-biginttobytes)
  - [`bigintToBytes()`](#bigintToBytes)
- [function `bigintFromI64`](#function-bigintfromi64)
  - [`bigintFromI64()`](#bigintFromI64)
- [function `bigintAdd`](#function-bigintadd)
  - [`bigintAdd()`](#bigintAdd)
- [function `bigintSubtract`](#function-bigintsubtract)
  - [`bigintSubtract()`](#bigintSubtract)
- [function `bigintMultiply`](#function-bigintmultiply)
  - [`bigintMultiply()`](#bigintMultiply)
- [function `bigintDivMod`](#function-bigintdivmod)
  - [`bigintDivMod()`](#bigintDivMod)
- [function `bigintModExp`](#function-bigintmodexp)
  - [`bigintModExp()`](#bigintModExp)

## Imports

- `uranite.encoding.binary`
  - `copyBytes`
- `uranite.io.syscall`
  - `readByteAt`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `LIMB_BITS`

## const `LIMB_MASK`

## const `LIMB_SIZE`

## const `MAX_LIMBS`

## class `BigInt`

Arbitrary-precision unsigned integer stored as an array of 32-bit limbs in little-endian order. Supports up to MAX_LIMBS limbs (8192 bits), sufficient for RSA-4096.

### Fields

| Name | Type | Access |
|------|------|--------|
| `limbsAddress` | `I64` | public |
| `limbCount` | `I64` | public |

### Methods

#### `function BigInt( self, I64 limbCount ) -> Void`

Construct a zero-initialized BigInt with the given limb count.

**Parameters**:

- `limbCount` (`I64`)
- `Number of 32-bit limbs.`

#### `function setLimb( self, I64 index, I64 value ) -> Void`

Set a limb at the given index.

**Parameters**:

- `index` (`I64`)
- `Limb index` (`0 = least significant`)
- `value` (`I64`)
- `32-bit limb value.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getLimb( self, I64 index ) -> I64`

Get a limb at the given index.

**Parameters**:

- `index` (`I64`)
- `Limb index.`

**Returns**: — I64:
32-bit limb value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isZero( self ) -> Boolean`

Check if this BigInt is zero.

**Returns**: — Boolean:
True if all limbs are zero.

**Complexity**:
- Time: `O(n) where n is limbCount`
- Space: `O(1)`

#### `function effectiveLimbs( self ) -> I64`

Count of significant limbs (excluding leading zeros).

**Returns**: — I64:
Number of significant limbs (minimum 1).

**Complexity**:
- Time: `O(n) where n is limbCount`
- Space: `O(1)`

#### `function bitLength( self ) -> I64`

Number of significant bits.

**Returns**: — I64:
Bit length of this integer.

**Complexity**:
- Time: `O(n) where n is limbCount`
- Space: `O(1)`

#### `function getBit( self, I64 bitIndex ) -> I64`

Get a single bit.

**Parameters**:

- `bitIndex` (`I64`)
- `Bit index` (`0 = LSB`)

**Returns**: — I64:
0 or 1.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function compare( self, BigInt other ) -> I64`

Compare this BigInt with another.

**Parameters**:

- `other` (`BigInt`)
- `BigInt to compare against.`

**Returns**: — I64:
-1 if self < other, 0 if equal, 1 if self > other.

**Complexity**:
- Time: `O(n) where n is max limb count`
- Space: `O(1)`

#### `function copyFrom( self, BigInt source ) -> Void`

Copy value from another BigInt.

**Parameters**:

- `source` (`BigInt`)
- `Source to copy from.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release limb storage.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `bigintFromBytes`

Create a BigInt from big-endian byte array.

**Parameters**:

- `dataAddress` (`I64`)
- `Address of big-endian bytes.`
- `dataLength` (`I64`)
- `Number of bytes.`
- `limbCount` (`I64`)
- `Limb count for the result.`

**Returns**: — BigInt:
New BigInt with the value.

**Complexity**:
- Time: `O(n) where n is dataLength`
- Space: `O(limbCount)`

### Methods

#### `function bigintFromBytes( I64 dataAddress, I64 dataLength, I64 limbCount ) -> BigInt`

Create a BigInt from big-endian byte array.

**Parameters**:

- `dataAddress` (`I64`)
- `Address of big-endian bytes.`
- `dataLength` (`I64`)
- `Number of bytes.`
- `limbCount` (`I64`)
- `Limb count for the result.`

**Returns**: — BigInt:
New BigInt with the value.

**Complexity**:
- Time: `O(n) where n is dataLength`
- Space: `O(limbCount)`

## function `bigintToBytes`

Write a BigInt as big-endian bytes.

**Parameters**:

- `value` (`BigInt`)
- `BigInt to serialize.`
- `outputAddress` (`I64`)
- `Destination address.`
- `outputLength` (`I64`)
- `Maximum output bytes.`

**Returns**: — I64:
Bytes written.

**Complexity**:
- Time: `O(n) where n is outputLength`
- Space: `O(1)`

### Methods

#### `function bigintToBytes( BigInt value, I64 outputAddress, I64 outputLength ) -> I64`

Write a BigInt as big-endian bytes.

**Parameters**:

- `value` (`BigInt`)
- `BigInt to serialize.`
- `outputAddress` (`I64`)
- `Destination address.`
- `outputLength` (`I64`)
- `Maximum output bytes.`

**Returns**: — I64:
Bytes written.

**Complexity**:
- Time: `O(n) where n is outputLength`
- Space: `O(1)`

## function `bigintFromI64`

Create a BigInt from a 64-bit integer.

**Parameters**:

- `value` (`I64`)
- `Integer value.`
- `limbCount` (`I64`)
- `Limb count.`

**Returns**: — BigInt:
New BigInt.

**Complexity**:
- Time: `O(limbCount)`
- Space: `O(limbCount)`

### Methods

#### `function bigintFromI64( I64 value, I64 limbCount ) -> BigInt`

Create a BigInt from a 64-bit integer.

**Parameters**:

- `value` (`I64`)
- `Integer value.`
- `limbCount` (`I64`)
- `Limb count.`

**Returns**: — BigInt:
New BigInt.

**Complexity**:
- Time: `O(limbCount)`
- Space: `O(limbCount)`

## function `bigintAdd`

result = augend + addend.

**Parameters**:

- `augend` (`BigInt`)
- `First operand.`
- `addend` (`BigInt`)
- `Second operand.`
- `result` (`BigInt`)
- `Destination for sum.`

**Complexity**:
- Time: `O(n) where n is limb count`
- Space: `O(1)`

### Methods

#### `function bigintAdd( BigInt augend, BigInt addend, BigInt result ) -> Void`

result = augend + addend.

**Parameters**:

- `augend` (`BigInt`)
- `First operand.`
- `addend` (`BigInt`)
- `Second operand.`
- `result` (`BigInt`)
- `Destination for sum.`

**Complexity**:
- Time: `O(n) where n is limb count`
- Space: `O(1)`

## function `bigintSubtract`

result = minuend - subtrahend. Assumes minuend >= subtrahend.

**Parameters**:

- `minuend` (`BigInt`)
- `Value to subtract from.`
- `subtrahend` (`BigInt`)
- `Value to subtract.`
- `result` (`BigInt`)
- `Destination for difference.`

**Complexity**:
- Time: `O(n) where n is limb count`
- Space: `O(1)`

### Methods

#### `function bigintSubtract( BigInt minuend, BigInt subtrahend, BigInt result ) -> Void`

result = minuend - subtrahend. Assumes minuend >= subtrahend.

**Parameters**:

- `minuend` (`BigInt`)
- `Value to subtract from.`
- `subtrahend` (`BigInt`)
- `Value to subtract.`
- `result` (`BigInt`)
- `Destination for difference.`

**Complexity**:
- Time: `O(n) where n is limb count`
- Space: `O(1)`

## function `bigintMultiply`

result = multiplicand * multiplier (schoolbook multiplication).

**Parameters**:

- `multiplicand` (`BigInt`)
- `First factor.`
- `multiplier` (`BigInt`)
- `Second factor.`
- `result` (`BigInt`)
- `Destination for product.`

**Complexity**:
- Time: `O(n^2) where n is limb count`
- Space: `O(1)`

### Methods

#### `function bigintMultiply( BigInt multiplicand, BigInt multiplier, BigInt result ) -> Void`

result = multiplicand * multiplier (schoolbook multiplication).

**Parameters**:

- `multiplicand` (`BigInt`)
- `First factor.`
- `multiplier` (`BigInt`)
- `Second factor.`
- `result` (`BigInt`)
- `Destination for product.`

**Complexity**:
- Time: `O(n^2) where n is limb count`
- Space: `O(1)`

## function `bigintDivMod`

quotient = dividend / divisor, remainder = dividend % divisor. Uses shift-and-subtract long division.

**Parameters**:

- `dividend` (`BigInt`)
- `Dividend.`
- `divisor` (`BigInt`)
- `Divisor` (`must be non-zero`)
- `quotient` (`BigInt`)
- `Destination for quotient.`
- `remainder` (`BigInt`)
- `Destination for remainder.`

**Complexity**:
- Time: `O(n^2) where n is bit length`
- Space: `O(n) for temporary`

### Methods

#### `function bigintDivMod( BigInt dividend, BigInt divisor, BigInt quotient, BigInt remainder ) -> Void`

quotient = dividend / divisor, remainder = dividend % divisor. Uses shift-and-subtract long division.

**Parameters**:

- `dividend` (`BigInt`)
- `Dividend.`
- `divisor` (`BigInt`)
- `Divisor` (`must be non-zero`)
- `quotient` (`BigInt`)
- `Destination for quotient.`
- `remainder` (`BigInt`)
- `Destination for remainder.`

**Complexity**:
- Time: `O(n^2) where n is bit length`
- Space: `O(n) for temporary`

## function `bigintModExp`

Compute base^exponent mod modulus via square-and-multiply.

**Parameters**:

- `base` (`BigInt`)
- `Base value.`
- `exponent` (`BigInt`)
- `Exponent.`
- `modulus` (`BigInt`)
- `Modulus.`

**Returns**: — BigInt:
Result of modular exponentiation.

**Complexity**:
- Time: `O(n^2 * e) where n is limb count, e is exponent bit length`
- Space: `O(n) for temporaries`

### Methods

#### `function bigintModExp( BigInt base, BigInt exponent, BigInt modulus ) -> BigInt`

Compute base^exponent mod modulus via square-and-multiply.

**Parameters**:

- `base` (`BigInt`)
- `Base value.`
- `exponent` (`BigInt`)
- `Exponent.`
- `modulus` (`BigInt`)
- `Modulus.`

**Returns**: — BigInt:
Result of modular exponentiation.

**Complexity**:
- Time: `O(n^2 * e) where n is limb count, e is exponent bit length`
- Space: `O(n) for temporaries`

