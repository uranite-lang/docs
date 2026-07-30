# uranite.collection.mutable-set

## Table of Contents

- [Imports](#imports)
- [interface `MutableSet`](#interface-mutableset)
  - [`add()`](#add)
  - [`addAll()`](#addAll)
  - [`discard()`](#discard)
  - [`remove()`](#remove)
  - [`removeAll()`](#removeAll)
  - [`retainAll()`](#retainAll)
  - [`clear()`](#clear)
  - [`addUnion()`](#addUnion)
  - [`retainIntersect()`](#retainIntersect)
  - [`removeDifference()`](#removeDifference)

## Imports

- `uranite.collection.sequence`
  - `Sequence`
- `uranite.collection.set`
  - `Set`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.void`
  - `Void`

## interface `MutableSet`<E>

**Implements**: `Set<E>`

Set that supports in-place addition and removal of elements.

This interface extends Set with mutating operations such as add, remove, discard, and set-theoretic updates like union, intersection, and difference, allowing concrete set implementations to modify their contents after construction.

### Methods

#### `function add( self, E element ) -> Boolean`

Add an element to this set.

**Parameters**:

- `element` (`E`)
- `The element to add.`

**Returns**: `Boolean` — True if the element was not already present and was added, False otherwise.

#### `function addAll( self, Sequence<E> elements ) -> Boolean`

Add all elements from a sequence to this set.

**Parameters**:

- `elements` (`Sequence<E>`)
- `The sequence of elements to add.`

**Returns**: `Boolean` — True if at least one new element was added, False if all were already present.

#### `function discard( self, E element ) -> Void`

Remove an element if present, without raising an error if absent.

**Parameters**:

- `element` (`E`)
- `The element to remove if it exists in this set.`

#### `function remove( self, E element ) -> Boolean`

Remove an element from this set.

**Parameters**:

- `element` (`E`)
- `The element to remove.`

**Returns**: `Boolean` — True if the element was found and removed, False otherwise.

#### `function removeAll( self, Sequence<E> elements ) -> Boolean`

Remove all elements that appear in the given sequence from this set.

**Parameters**:

- `elements` (`Sequence<E>`)
- `The sequence of elements to remove.`

**Returns**: `Boolean` — True if at least one element was removed, False otherwise.

#### `function retainAll( self, Sequence<E> elements ) -> Boolean`

Keep only elements that are present in the given sequence, removing all others.

**Parameters**:

- `elements` (`Sequence<E>`)
- `The sequence of elements to retain.`

**Returns**: `Boolean` — True if any elements were removed as a result, False otherwise.

#### `function clear( self ) -> Void`

Remove all elements from this set, leaving it empty. 

#### `function addUnion( self, Set<E> other ) -> Void`

Add all elements from another set into this set, performing an in-place union.

**Parameters**:

- `other` (`Set<E>`)
- `The set whose elements will be added.`

#### `function retainIntersect( self, Set<E> other ) -> Void`

Keep only elements that are also present in the other set, performing an in-place intersection.

**Parameters**:

- `other` (`Set<E>`)
- `The set to intersect with.`

#### `function removeDifference( self, Set<E> other ) -> Void`

Remove all elements found in the other set from this set, performing an in-place difference.

**Parameters**:

- `other` (`Set<E>`)
- `The set whose elements will be removed from this set.`

