# uranite.language.integer

## Table of Contents

- [Imports](#imports)
- [class `Integer`](#class-integer)
  - [`Integer()`](#Integer)
  - [`toInteger()`](#toInteger)
  - [`toString()`](#toString)

## Imports

- `uranite.language.int`
  - `Int`

## class `Integer`

**Extends**: `Int`

General-purpose integer type, maps to I32 on most platforms. Extends Int and provides a platform-natural integer width for common use cases.

### Methods

#### `function Integer( self, I32 value ) -> Void`

Construct a new Integer from the given I32 value, widening to I64 internally. 

#### `function toInteger( self ) -> I32`

Return this value narrowed back to a 32-bit integer. 

#### `function toString( self ) -> String`

Return the string representation of this Integer's numeric value. 

