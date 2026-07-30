# uranite.operators.negatable

## Table of Contents

- [interface `Negatable`](#interface-negatable)
  - [`negate()`](#negate)

## interface `Negatable`<T>

Interface for types that support unary negation (-). Implementing types must define how to produce the negated form of a value.

### Methods

#### `function negate( self ) -> T`

Return the negated value of self. 

