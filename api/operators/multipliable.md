# uranite.operators.multipliable

## Table of Contents

- [interface `Multipliable`](#interface-multipliable)
  - [`multiply()`](#multiply)

## interface `Multipliable`<T>

Interface for types that support the multiplication (*) operator. Implementing types must define how two values of the same type are multiplied together.

### Methods

#### `function multiply( self, T other ) -> T`

Return the result of multiplying self by the other value. 

