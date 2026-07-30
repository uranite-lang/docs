# uranite.memory.memory

## Table of Contents

- [Imports](#imports)
- [class `Memory`](#class-memory)
  - [`Memory()`](#Memory)
  - [`free()`](#free)
  - [`get()`](#get)
  - [`set()`](#set)
  - [`copyTo()`](#copyTo)

## Imports

- `uranite.language.int`
  - `Int`
- `uranite.language.void`
  - `Void`

## class `Memory`<T>

Raw typed pointer abstraction that serves as a compiler intrinsic for direct memory management. Each method translates directly to LLVM instructions without any runtime overhead.

### Methods

#### `function Memory( self, Int capacity ) -> Void`

Allocate a contiguous memory block for the given number of elements.

Translates to a call to malloc(capacity * sizeof(T)) at the LLVM level.

**Parameters**:

- `capacity` (`Int`)
- `The number of elements of type T to allocate space for.`

#### `function free( self ) -> Void`

Release the memory block back to the allocator.

Translates to a call to free(pointer) at the LLVM level. After calling free, the Memory instance must not be used again.

#### `function get( self, Int index ) -> T`

Read a value at the given element offset.

Translates to a getelementptr followed by a load instruction at the LLVM level.

**Parameters**:

- `index` (`Int`)
- `The zero-based element offset to read from.`

**Returns**: `T` — The value stored at the given index.

#### `function set( self, Int index, T value ) -> Void`

Write a value at the given element offset.

Translates to a getelementptr followed by a store instruction at the LLVM level.

**Parameters**:

- `index` (`Int`)
- `The zero-based element offset to write to.`
- `value` (`T`)
- `The value to store at the given index.`

#### `function copyTo( self, Memory<T> destination, Int length ) -> Void`

Copy elements from this memory block to a destination memory block.

Translates to an llvm.memcpy intrinsic call for bulk copy optimization.

**Parameters**:

- `destination` (`Memory<T>`)
- `The target memory block to copy elements into.`
- `length` (`Int`)
- `The number of elements to copy.`

