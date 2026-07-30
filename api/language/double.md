# uranite.language.double

## Table of Contents

- [Imports](#imports)
- [class `Double`](#class-double)
  - [`Double()`](#Double)
  - [`toDouble()`](#toDouble)
  - [`toString()`](#toString)

## Imports

- `uranite.language.float`
  - `Float`

## class `Double`

**Extends**: `Float`

64-bit double-precision floating-point type. Extends Float and serves as an alias for F64, providing double-precision arithmetic.

### Methods

#### `function Double( self, F64 value ) -> Void`

Construct a new Double from the given F64 value. 

#### `function toDouble( self ) -> F64`

Return the underlying F64 value of this Double. 

#### `function toString( self ) -> String`

Return the string representation of this Double's numeric value. 

