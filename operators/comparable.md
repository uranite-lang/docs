# uranite.operators.comparable

## Table of Contents

- [interface `Comparable`](#interface-comparable)
  - [`compareTo()`](#compareTo)
  - [`lessThan()`](#lessThan)
  - [`greaterThan()`](#greaterThan)
  - [`lessOrEqual()`](#lessOrEqual)
  - [`greaterOrEqual()`](#greaterOrEqual)

## interface `Comparable`<T>

Interface for types that support ordering comparisons (<, >, <=, >=). Implementing types must define a total ordering over their values.

### Methods

#### `function compareTo( self, T other ) -> I32`

Return a negative, zero, or positive I32 for less-than, equal, or greater-than respectively. 

#### `function lessThan( self, T other ) -> Boolean`

Return True if self is strictly less than other. 

#### `function greaterThan( self, T other ) -> Boolean`

Return True if self is strictly greater than other. 

#### `function lessOrEqual( self, T other ) -> Boolean`

Return True if self is less than or equal to other. 

#### `function greaterOrEqual( self, T other ) -> Boolean`

Return True if self is greater than or equal to other. 

