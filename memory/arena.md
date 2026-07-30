# uranite.memory.arena

## Table of Contents

- [Imports](#imports)
- [class `Arena`](#class-arena)
  - [`Arena()`](#Arena)
  - [`alloc()`](#alloc)
  - [`freeAll()`](#freeAll)
  - [`destroy()`](#destroy)
  - [`count()`](#count)
  - [`capacity()`](#capacity)

## Imports

- `uranite.language.int`
  - `Int`
- `uranite.language.void`
  - `Void`

## class `Arena`<T>

Bulk arena allocator that allocates a contiguous memory region upfront and sub-allocates individual elements via pointer bumping. All allocations can be freed in constant time by resetting the bump pointer.

### Methods

#### `function Arena( self, Int capacity ) -> Void`

Construct a new arena with the given capacity.

Allocates a contiguous memory block of capacity * sizeof(T) bytes and initializes the bump pointer to zero.

**Parameters**:

- `capacity` (`Int`)
- `The maximum number of elements the arena can hold.`

#### `function alloc( self ) -> T`

Bump-allocate one element from the arena.

Advances the internal bump pointer by one slot and returns a pointer to the newly allocated slot. Behavior is undefined if the arena is full.

**Returns**: `T` — A pointer to the newly allocated element slot.

#### `function freeAll( self ) -> Void`

Reset the bump pointer to zero, logically freeing all allocations.

This operation runs in constant time regardless of the number of allocated elements. The underlying memory block is retained for reuse.

#### `function destroy( self ) -> Void`

Free the underlying memory block allocated by the arena.

After calling destroy, the arena must not be used again. This releases all memory back to the system.

#### `function count( self ) -> Int`

Return the number of currently allocated element slots in the arena. 

#### `function capacity( self ) -> Int`

Return the total capacity of the arena in number of element slots. 

