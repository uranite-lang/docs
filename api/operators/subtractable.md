# uranite.operators.subtractable

## Table of Contents

- [interface `Subtractable`](#interface-subtractable)
  - [`subtract()`](#subtract)

## interface `Subtractable`<T>

Interface for types that support the subtraction (-) operator. Implementing types must define how to subtract one value from another of the same type.

### Methods

#### `function subtract( self, T other ) -> T`

Return the result of subtracting the other value from self. 

