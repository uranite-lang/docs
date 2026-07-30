# uranite.collection.sequence

## Table of Contents

- [Imports](#imports)
- [interface `Sequence`](#interface-sequence)
  - [`indexOf()`](#indexOf)
  - [`lastIndexOf()`](#lastIndexOf)
  - [`slice()`](#slice)
  - [`subSequence()`](#subSequence)
  - [`first()`](#first)
  - [`last()`](#last)
  - [`reversed()`](#reversed)
  - [`sorted()`](#sorted)
  - [`map()`](#map)
  - [`forEach()`](#forEach)
  - [`zip()`](#zip)

## Imports

- `uranite.collection.collection`
  - `Collection`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.callable`
  - `Callable`
- `uranite.language.void`
  - `Void`
- `uranite.operators.indexable`
  - `Indexable`

## interface `Sequence`<E>

**Implements**: `Collection<E>`, `Indexable<Int, E>`

Ordered collection with index-based access to elements.

A Sequence maintains a defined ordering of its elements, allowing retrieval, searching, slicing, and transformation operations based on positional indices. Inherits get/set/remove/contains from Indexable<Int, E>.

### Methods

#### `function indexOf( self, E element ) -> Int`

Return the index of the first occurrence of the given element, or -1 if the element is not present.

**Parameters**:

- `element` (`E`)
- `The element to search for.`

**Returns**: `Int` — The zero-based index of the first occurrence, or -1 if absent.

#### `function lastIndexOf( self, E element ) -> Int`

Return the index of the last occurrence of the given element, or -1 if the element is not present.

**Parameters**:

- `element` (`E`)
- `The element to search for.`

**Returns**: `Int` — The zero-based index of the last occurrence, or -1 if absent.

#### `function slice( self, Int start, Int end ) -> Sequence<E>`

Return a new sequence containing elements from the start index up to but not including the end index.

**Parameters**:

- `start` (`Int`)
- `The inclusive start index.`
- `end` (`Int`)
- `The exclusive end index.`

**Returns**: `Sequence<E>` — A new sequence containing the specified range of elements.

#### `function subSequence( self, Int start, Int end ) -> Sequence<E>`

Return a sub-sequence containing elements between the start and end indices.

**Parameters**:

- `start` (`Int`)
- `The inclusive start index.`
- `end` (`Int`)
- `The exclusive end index.`

**Returns**: `Sequence<E>` — A new sequence containing the specified range of elements.

#### `property first( self ) -> E`

`property` 

Return the first element of this sequence. 

#### `property last( self ) -> E`

`property` 

Return the last element of this sequence. 

#### `property reversed( self ) -> Sequence<E>`

`property` 

Return a new sequence with the elements in reverse order. 

#### `function sorted( self, <type> comparator ) -> Sequence<E>`

Return a new sorted sequence using the provided comparator function.

**Parameters**:

- `comparator` (`Callable<Int,<E,E>>`)
- `A function that takes two elements and returns a negative, zero, or positive`
- `Int indicating their relative order.`

**Returns**: `Sequence<E>` — A new sequence with elements sorted according to the comparator.

#### `function map( self, <type> transform ) -> Sequence<E>`

Return a new sequence with the given transform function applied to each element.

**Parameters**:

- `transform` (`Callable<E,<E>>`)
- `A function that takes an element and returns a transformed element.`

**Returns**: `Sequence<E>` — A new sequence containing the transformed elements.

#### `function forEach( self, <type> action ) -> Void`

Execute the given action for each element in this sequence.

**Parameters**:

- `action` (`Callable<Void,<E>>`)
- `A function to invoke with each element.`

#### `function zip( self, Sequence<E> other ) -> Sequence<E>`

Interleave elements from this sequence and another sequence into a new sequence.

**Parameters**:

- `other` (`Sequence<E>`)
- `The sequence whose elements will be interleaved with this sequence.`

**Returns**: `Sequence<E>` — A new sequence with elements alternating between this and the other sequence.

