# uranite.collection.mutable-sequence

## Table of Contents

- [Imports](#imports)
- [interface `MutableSequence`](#interface-mutablesequence)
  - [`add()`](#add)
  - [`addAll()`](#addAll)
  - [`insert()`](#insert)
  - [`removeAt()`](#removeAt)
  - [`removeFirst()`](#removeFirst)
  - [`removeLast()`](#removeLast)
  - [`removeElement()`](#removeElement)
  - [`clear()`](#clear)
  - [`sort()`](#sort)
  - [`reverse()`](#reverse)
  - [`swap()`](#swap)
  - [`fill()`](#fill)
  - [`resize()`](#resize)

## Imports

- `uranite.collection.sequence`
  - `Sequence`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.void`
  - `Void`

## interface `MutableSequence`<E>

**Implements**: `Sequence<E>`

Sequence that supports in-place modification of elements.

This interface extends Sequence with mutating operations such as add, insert, remove, sort, and reverse, allowing concrete sequence implementations to modify their contents after construction.

### Methods

#### `function add( self, E element ) -> Void`

Append an element to the end of this sequence.

**Parameters**:

- `element` (`E`)
- `The element to append.`

#### `function addAll( self, Sequence<E> elements ) -> Void`

Append all elements from another sequence to the end of this sequence.

**Parameters**:

- `elements` (`Sequence<E>`)
- `The sequence whose elements will be appended.`

#### `function insert( self, Int index, E element ) -> Void`

Insert an element at the given index, shifting all subsequent elements to the right.

**Parameters**:

- `index` (`Int`)
- `The position at which to insert the element.`
- `element` (`E`)
- `The element to insert.`

#### `function removeAt( self, Int index ) -> E`

Remove and return the element at the given index, shifting subsequent elements to the left.

**Parameters**:

- `index` (`Int`)
- `The position of the element to remove.`

**Returns**: `E` — The element that was removed.

#### `function removeFirst( self ) -> E`

Remove and return the first element of this sequence.

**Returns**: `E` — The element that was removed from the beginning of the sequence.

#### `function removeLast( self ) -> E`

Remove and return the last element of this sequence.

**Returns**: `E` — The element that was removed from the end of the sequence.

#### `function removeElement( self, E element ) -> Boolean`

Remove the first occurrence of the given element from this sequence.

**Parameters**:

- `element` (`E`)
- `The element to search for and remove.`

**Returns**: `Boolean` — True if the element was found and removed, False otherwise.

#### `function clear( self ) -> Void`

Remove all elements from this sequence, leaving it empty. 

#### `function sort( self ) -> Void`

Sort all elements in this sequence in-place using their natural ordering. 

#### `function reverse( self ) -> Void`

Reverse the order of all elements in this sequence in-place. 

#### `function swap( self, Int indexA, Int indexB ) -> Void`

Swap the elements at two indices in this sequence.

**Parameters**:

- `indexA` (`Int`)
- `The position of the first element to swap.`
- `indexB` (`Int`)
- `The position of the second element to swap.`

#### `function fill( self, E element ) -> Void`

Set all elements in this sequence to the given value.

**Parameters**:

- `element` (`E`)
- `The value to assign to every position in the sequence.`

#### `function resize( self, Int newSize ) -> Void`

Change the logical size of this sequence.

If the new size is larger than the current size, additional positions are added with default values. If the new size is smaller, excess elements are removed.

**Parameters**:

- `newSize` (`Int`)
- `The desired new size of the sequence.`

