# uranite.language.long

## Table of Contents

- [Imports](#imports)
- [class `Long`](#class-long)
  - [`Long()`](#Long)
  - [`toLong()`](#toLong)
  - [`toString()`](#toString)

## Imports

- `uranite.language.int`
  - `Int`

## class `Long`

**Extends**: `Int`

64-bit signed integer type. Extends Int and serves as a named alias for I64, providing the full 64-bit signed range.

### Methods

#### `function Long( self, I64 value ) -> Void`

Construct a new Long from the given I64 value. 

#### `function toLong( self ) -> I64`

Return the underlying I64 value. 

#### `function toString( self ) -> String`

Return the string representation of this Long's numeric value. 

