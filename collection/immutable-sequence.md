# uranite.collection.immutable-sequence

## Table of Contents

- [Imports](#imports)
- [interface `ImmutableSequence`](#interface-immutablesequence)
  - [`with()`](#with)
  - [`appended()`](#appended)
  - [`prepended()`](#prepended)
  - [`concatenated()`](#concatenated)
  - [`filtered()`](#filtered)
  - [`dropped()`](#dropped)
  - [`taken()`](#taken)

## Imports

- `uranite.collection.sequence`
  - `Sequence`
- `uranite.language.callable`
  - `Callable`

## interface `ImmutableSequence`<E>

**Implements**: `Sequence<E>`

Persistent sequence where all mutation operations return new copies rather than modifying the original, ensuring immutability.

### Methods

#### `function with( self, Int index, E element ) -> ImmutableSequence<E>`

Return a new sequence with the element at the given index replaced. 

#### `function appended( self, E element ) -> ImmutableSequence<E>`

Return a new sequence with the given element appended to the end. 

#### `function prepended( self, E element ) -> ImmutableSequence<E>`

Return a new sequence with the given element prepended to the front. 

#### `function concatenated( self, Sequence<E> other ) -> ImmutableSequence<E>`

Return a new sequence with the other sequence concatenated to the end. 

#### `function filtered( self, <type> predicate ) -> ImmutableSequence<E>`

Return a new sequence containing only elements that match the predicate. 

#### `function dropped( self, Int count ) -> ImmutableSequence<E>`

Return a new sequence with the first count elements removed. 

#### `function taken( self, Int count ) -> ImmutableSequence<E>`

Return a new sequence containing only the first count elements. 

