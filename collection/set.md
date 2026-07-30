# uranite.collection.set

## Table of Contents

- [Imports](#imports)
- [interface `Set`](#interface-set)
  - [`isSubsetOf()`](#isSubsetOf)
  - [`isSupersetOf()`](#isSupersetOf)
  - [`isDisjoint()`](#isDisjoint)
  - [`union()`](#union)
  - [`intersect()`](#intersect)
  - [`difference()`](#difference)
  - [`symmetricDifference()`](#symmetricDifference)

## Imports

- `uranite.collection.collection`
  - `Collection`
- `uranite.language.boolean`
  - `Boolean`

## interface `Set`<E>

**Implements**: `Collection<E>`

Unordered collection of unique elements.

A Set guarantees that no two elements are equal according to their equality comparison. It provides standard set-theoretic operations including subset checks, union, intersection, difference, and symmetric difference.

### Methods

#### `function isSubsetOf( self, Set<E> other ) -> Boolean`

Check whether all elements of this set are contained in the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to check against.`

**Returns**: `Boolean` — True if every element of this set is also in the other set, False otherwise.

#### `function isSupersetOf( self, Set<E> other ) -> Boolean`

Check whether this set contains all elements of the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to check against.`

**Returns**: `Boolean` — True if every element of the other set is also in this set, False otherwise.

#### `function isDisjoint( self, Set<E> other ) -> Boolean`

Check whether this set shares no elements with the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to check against.`

**Returns**: `Boolean` — True if the two sets have no elements in common, False otherwise.

#### `function union( self, Set<E> other ) -> Set<E>`

Return a new set containing all elements from both this set and the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to combine with.`

**Returns**: `Set<E>` — A new set containing all unique elements from both sets.

#### `function intersect( self, Set<E> other ) -> Set<E>`

Return a new set containing only elements present in both this set and the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to intersect with.`

**Returns**: `Set<E>` — A new set containing only the elements common to both sets.

#### `function difference( self, Set<E> other ) -> Set<E>`

Return a new set containing elements in this set but not in the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set whose elements will be excluded.`

**Returns**: `Set<E>` — A new set containing elements present in this set but absent from the other.

#### `function symmetricDifference( self, Set<E> other ) -> Set<E>`

Return a new set containing elements that are in either set but not in both.

**Parameters**:

- `other` (`Set<E>`)
- `The set to compute symmetric difference with.`

**Returns**: `Set<E>` — A new set containing elements exclusive to one of the two sets.

