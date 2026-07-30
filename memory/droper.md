# uranite.memory.droper

## Table of Contents

- [interface `Droper`](#interface-droper)
  - [`drop()`](#drop)

## interface `Droper`

Interface for types that need explicit resource cleanup.

Implementing types must provide a drop method that releases any owned resources such as memory, file descriptors, locks, or hardware registers. Called manually when an object's lifetime ends.

### Methods

#### `function drop( self ) -> Void`

Release all resources owned by this object.

