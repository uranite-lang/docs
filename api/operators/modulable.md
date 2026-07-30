# uranite.operators.modulable

## Table of Contents

- [interface `Modulable`](#interface-modulable)
  - [`modulo()`](#modulo)

## interface `Modulable`<T>

Interface for types that support the modulo (%) operator. Implementing types must define how to compute the remainder of division.

### Methods

#### `function modulo( self, T other ) -> T`

Return the remainder of dividing self by the other value. 

