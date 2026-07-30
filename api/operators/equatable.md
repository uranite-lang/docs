# uranite.operators.equatable

## Table of Contents

- [interface `Equatable`](#interface-equatable)
  - [`equals()`](#equals)
  - [`notEquals()`](#notEquals)

## interface `Equatable`<T>

Interface for types that support equality comparison (== and !=). Implementing types must define structural or semantic equality for their values.

### Methods

#### `function equals( self, T other ) -> Boolean`

Return True if self is equal to the other value. 

#### `function notEquals( self, T other ) -> Boolean`

Return True if self is not equal to the other value. 

