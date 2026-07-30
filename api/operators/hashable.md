# uranite.operators.hashable

## Table of Contents

- [interface `Hashable`](#interface-hashable)
  - [`hash()`](#hash)

## interface `Hashable`

Interface for types that can produce an integer hash code. Required for use as keys in hash-based collections such as HashMap and HashSet.

### Methods

#### `function hash( self ) -> I64`

Return the integer hash code for this value, suitable for use in hash-based collections. 

