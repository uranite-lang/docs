# uranite.collection.hash-set

## Table of Contents

- [Imports](#imports)
- [class `HashSet`](#class-hashset)
  - [`HashSet()`](#HashSet)
  - [`HashSet()`](#HashSet)
  - [`HashSet()`](#HashSet)
  - [`add()`](#add)
  - [`addAll()`](#addAll)
  - [`discard()`](#discard)
  - [`removeAll()`](#removeAll)
  - [`retainAll()`](#retainAll)
  - [`addUnion()`](#addUnion)
  - [`retainIntersect()`](#retainIntersect)
  - [`removeDifference()`](#removeDifference)
  - [`isSubsetOf()`](#isSubsetOf)
  - [`isSupersetOf()`](#isSupersetOf)
  - [`isDisjoint()`](#isDisjoint)
  - [`union()`](#union)
  - [`intersect()`](#intersect)
  - [`difference()`](#difference)
  - [`symmetricDifference()`](#symmetricDifference)
  - [`append()`](#append)
  - [`clear()`](#clear)
  - [`copy()`](#copy)
  - [`empty()`](#empty)
  - [`equals()`](#equals)
  - [`exists()`](#exists)
  - [`filter()`](#filter)
  - [`forEach()`](#forEach)
  - [`length()`](#length)
  - [`remove()`](#remove)
  - [`iterator()`](#iterator)
  - [`toString()`](#toString)
  - [`size()`](#size)
  - [`isEmpty()`](#isEmpty)
  - [`isNotEmpty()`](#isNotEmpty)
  - [`toList()`](#toList)
  - [`any()`](#any)
  - [`all()`](#all)
  - [`noneMatch()`](#noneMatch)
  - [`destroy()`](#destroy)
- [class `HashSetIterator`](#class-hashsetiterator)
  - [`HashSetIterator()`](#HashSetIterator)
  - [`has()`](#has)
  - [`next()`](#next)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.collection.collection`
  - `Collection`
- `uranite.collection.mutable-set`
  - `MutableSet`
- `uranite.collection.sequence`
  - `Sequence`
- `uranite.collection.set`
  - `Set`
- `uranite.iterators.iterator`
  - `Iterator`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.callable`
  - `Callable`
- `uranite.language.string`
  - `String`
- `uranite.language.void`
  - `Void`
- `uranite.memory.memory`
  - `Memory`

## class `HashSet`<E>

**Implements**: `MutableSet<E>`

Hash-based set using open addressing with linear probing for unique elements.

Elements are hashed to determine bucket placement. Collisions are resolved by linear probing, and deleted slots are marked as tombstones (status 2) to maintain probe chain integrity. The table automatically grows when the load factor exceeds 75%.

### Fields

| Name | Type | Access |
|------|------|--------|
| `data` | `Memory<E>` | private |
| `status` | `Memory<Int>` | private |
| `count` | `Int` | private |
| `capacity` | `Int` | private |

### Methods

#### `function HashSet( self ) -> Void`

Create an empty set with a default initial capacity of 16 buckets.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function HashSet( self, Int initialCapacity ) -> Void`

Create an empty set with the specified initial capacity.

**Parameters**:

- `initialCapacity` (`Int`)
- `The number of buckets to pre-allocate.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function HashSet( self, Memory<E> elements, Int elementCount ) -> Void`

#### `function computeIndex( self, E element ) -> Int`

Compute the bucket index from the element's hash code, wrapping negative values to ensure a positive index within the capacity range.

**Parameters**:

- `element` (`E`)
- `The element to compute the index for.`

**Returns**: — Int:
The non-negative bucket index.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function probe( self, E element ) -> Int`

Perform linear probing to find the slot for the given element, reusing the first deleted tombstone encountered during the probe.

**Parameters**:

- `element` (`E`)
- `The element to probe for.`

**Returns**: — Int:
The slot index where the element should be placed, or -1 if the
table is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function grow( self ) -> Void`

Double the table capacity and rehash all existing elements into the new buckets.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function add( self, E element ) -> Boolean`

Add an element to the set. If the element is already present, no change is made. The table grows automatically when the load factor exceeds 75%.

**Parameters**:

- `element` (`E`)
- `The element to add.`

**Returns**: — Boolean:
True if the element was added, False if it was already present.

**Complexity**:
- Time: `O(1) amortized, O(n) worst case`
- Space: `O(1) amortized`

#### `function addAll( self, Sequence<E> elements ) -> Boolean`

Add all elements from a sequence to the set.

**Parameters**:

- `elements` (`Sequence<E>`)
- `The sequence of elements to add.`

**Returns**: — Boolean:
True if at least one element was added, False if all were already present.

**Complexity**:
- Time: `O(m) where m is elements.length`
- Space: `O(1) amortized`

#### `function discard( self, E element ) -> Void`

Remove the element if present, with no error if the element is absent.

**Parameters**:

- `element` (`E`)
- `The element to discard.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function removeAll( self, Sequence<E> elements ) -> Boolean`

Remove all given elements from the set.

**Parameters**:

- `elements` (`Sequence<E>`)
- `The sequence of elements to remove.`

**Returns**: — Boolean:
True if at least one element was removed.

**Complexity**:
- Time: `O(m * n) where m is elements.length`
- Space: `O(1)`

#### `function retainAll( self, Sequence<E> elements ) -> Boolean`

Keep only elements that are present in the given sequence, removing all others.

**Parameters**:

- `elements` (`Sequence<E>`)
- `The sequence of elements to retain.`

**Returns**: — Boolean:
True if at least one element was removed.

**Complexity**:
- Time: `O(n + m) where m is elements.length`
- Space: `O(m)`

#### `function addUnion( self, Set<E> other ) -> Void`

Add all elements from the other set into this set, performing an in-place union operation.

**Parameters**:

- `other` (`Set<E>`)
- `The set whose elements are to be added.`

**Complexity**:
- Time: `O(m) amortized where m is other.size`
- Space: `O(1) amortized`

#### `function retainIntersect( self, Set<E> other ) -> Void`

Keep only elements that are also in the other set, performing an in-place intersection operation.

**Parameters**:

- `other` (`Set<E>`)
- `The set to intersect with.`

**Complexity**:
- Time: `O(n * m) where m is other.size`
- Space: `O(1)`

#### `function removeDifference( self, Set<E> other ) -> Void`

Remove all elements that are found in the other set, performing an in-place difference operation.

**Parameters**:

- `other` (`Set<E>`)
- `The set whose elements are to be removed from this set.`

**Complexity**:
- Time: `O(m * n) where m is other.size`
- Space: `O(1)`

#### `function isSubsetOf( self, Set<E> other ) -> Boolean`

Check whether all elements of this set are contained in the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to check against.`

**Returns**: — Boolean:
True if every element in this set is also in the other set.

**Complexity**:
- Time: `O(n * m) where m is other.size`
- Space: `O(1)`

#### `function isSupersetOf( self, Set<E> other ) -> Boolean`

Check whether this set contains all elements of the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to check against.`

**Returns**: — Boolean:
True if every element in the other set is also in this set.

**Complexity**:
- Time: `O(m * n) where m is other.size`
- Space: `O(1)`

#### `function isDisjoint( self, Set<E> other ) -> Boolean`

Check whether this set shares no elements with the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to check against.`

**Returns**: — Boolean:
True if there are no elements in common.

**Complexity**:
- Time: `O(n * m) where m is other.size`
- Space: `O(1)`

#### `function union( self, Set<E> other ) -> Set<E>`

Return a new set containing elements from both this set and the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to union with.`

**Returns**: — Set<E>:
A new set containing all elements from both sets.

**Complexity**:
- Time: `O(n + m) where m is other.size`
- Space: `O(n + m)`

#### `function intersect( self, Set<E> other ) -> Set<E>`

Return a new set containing only elements present in both this set and the other set.

**Parameters**:

- `other` (`Set<E>`)
- `The set to intersect with.`

**Returns**: — Set<E>:
A new set containing only the common elements.

**Complexity**:
- Time: `O(n * m) where m is other.size`
- Space: `O(min(n, m))`

#### `function difference( self, Set<E> other ) -> Set<E>`

Return a new set with elements that are in this set but not in the other.

**Parameters**:

- `other` (`Set<E>`)
- `The set to subtract.`

**Returns**: — Set<E>:
A new set containing elements unique to this set.

**Complexity**:
- Time: `O(n * m) where m is other.size`
- Space: `O(n)`

#### `function symmetricDifference( self, Set<E> other ) -> Set<E>`

Return a new set with elements that are in either set but not in both.

**Parameters**:

- `other` (`Set<E>`)
- `The set to compute the symmetric difference with.`

**Returns**: — Set<E>:
A new set containing elements exclusive to one set or the other.

**Complexity**:
- Time: `O(n * m)`
- Space: `O(n + m)`

#### `function append( self, E element ) -> Collection<E>`

Add an element and return self for chaining.

**Parameters**:

- `element` (`E`)
- `The element to add.`

**Returns**: — Collection<E>:
This set instance.

**Complexity**:
- Time: `O(1) amortized, O(n) worst case`
- Space: `O(1) amortized`

#### `function clear( self ) -> Void`

Remove all elements from the set by resetting all slot statuses to empty.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property copy( self ) -> Collection<E>`

`property` 

Return a shallow copy of this set.

**Returns**: — Collection<E>:
A new HashSet containing the same elements.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property empty( self ) -> Boolean`

`property` 

Check whether the set contains no elements.

**Returns**: `Boolean` — True if the set has zero elements, False otherwise.

#### `function equals( self, Object object ) -> Boolean`

Compare this set with another object for structural equality.

**Parameters**:

- `object` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is a HashSet with the same elements.

**Complexity**:
- Time: `O(n * m)`
- Space: `O(1)`

#### `function exists( self, E element ) -> Boolean`

Check whether the given element exists in the set.

**Parameters**:

- `element` (`E`)
- `The element to search for.`

**Returns**: — Boolean:
True if the element is found, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function filter( self, <type> callback ) -> Collection<E>`

Return a new set containing only elements that match the predicate.

**Parameters**:

- `callback` (`Callable<Boolean,<E>>`)
- `A predicate function that returns True for elements to include.`

**Returns**: — Collection<E>:
A new HashSet with only the matching elements.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function forEach( self, <type> action ) -> Void`

Execute an action for each element in the set.

**Parameters**:

- `action` (`Callable<Void, <E>>`)
- `The action to execute with each element.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property length( self ) -> Int`

`property` 

Return the number of elements currently in the set. 

#### `function remove( self, E element ) -> Boolean`

Remove the given element from the set.

**Parameters**:

- `element` (`E`)
- `The element to remove.`

**Returns**: — Boolean:
True if the element was found and removed, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function removeElement( self, E element ) -> Boolean`

Remove an element by marking its slot as a tombstone (status 2) to maintain probe chain integrity.

**Parameters**:

- `element` (`E`)
- `The element to remove.`

**Returns**: — Boolean:
True if the element was found and removed, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property iterator( self ) -> Iterator<E>`

`property` 

Return an iterator over the elements of this set.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toString( self ) -> String`

Return a string representation of the set in the format {elem1, elem2, ...}.

**Returns**: — String:
The formatted string representation.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property size( self ) -> Int`

`property` 

Return the number of elements in the set.

Alias for the length property to match common collection conventions.

**Returns**: — Int:
The element count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property isEmpty( self ) -> Boolean`

`property` 

Test whether the set contains no elements.

**Returns**: — Boolean:
True if the set has zero elements.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property isNotEmpty( self ) -> Boolean`

`property` 

Test whether the set contains at least one element.

**Returns**: — Boolean:
True if the set has one or more elements.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toList( self ) -> ArrayList<E>`

Convert the set to an ArrayList preserving iteration order.

**Returns**: — ArrayList<E>:
New list containing all elements of the set.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function any( self, <type> predicate ) -> Boolean`

Test whether at least one element satisfies the predicate.

Short-circuits on the first match.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns True for a matching element.`

**Returns**: — Boolean:
True if any element satisfies the predicate.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function all( self, <type> predicate ) -> Boolean`

Test whether every element satisfies the predicate.

Short-circuits on the first non-matching element.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns True for a conforming element.`

**Returns**: — Boolean:
True if all elements satisfy the predicate.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function noneMatch( self, <type> predicate ) -> Boolean`

Test whether no element satisfies the predicate.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns True for a matching element.`

**Returns**: — Boolean:
True if no element satisfies the predicate.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release all heap-allocated memory buffers held by this set.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `HashSetIterator`<E>

**Implements**: `Iterator<E>`

Iterator that traverses occupied slots in a HashSet's data array.

Skips over empty and tombstone slots, yielding only elements from active entries.

### Fields

| Name | Type | Access |
|------|------|--------|
| `data` | `Memory<E>` | private |
| `status` | `Memory<Int>` | private |
| `capacity` | `Int` | private |
| `position` | `Int` | private |

### Methods

#### `function HashSetIterator( self, Memory<E> data, Memory<Int> status, Int capacity ) -> Void`

Create an iterator over the given data array, starting at the first occupied slot.

**Parameters**:

- `data` (`Memory<E>`)
- `The element storage buffer to iterate over.`
- `status` (`Memory<Int>`)
- `The slot status array` (`0=empty, 1=occupied, 2=deleted`)
- `capacity` (`Int`)
- `The total number of slots in the table.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property has( self ) -> Boolean`

`property` 

Check whether more elements remain to be iterated.

**Returns**: `Boolean` — True if the iterator has not yet reached the end of the table.

#### `property next( self ) -> E`

`property` 

Return the current element and advance to the next occupied slot.

**Returns**: — E:
The element at the current position before advancing.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

