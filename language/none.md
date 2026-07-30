# uranite.language.none

## Table of Contents

- [class `NoneType`](#class-nonetype)
  - [`toString()`](#toString)
  - [`equals()`](#equals)

## class `NoneType`

Type of the None singleton, representing the absence of a value. Used as the underlying type for nullable expressions that have no value.

### Methods

#### `function toString( self ) -> String`

Return the string "None" representing this absent value. 

#### `function equals( self, ?NoneType other ) -> Boolean`

Return True, since all NoneType instances are considered equal. 

