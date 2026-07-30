# uranite.collection.array-list

## Table of Contents

- [Imports](#imports)
- [class `ArrayList`](#class-arraylist)
  - [`ArrayList()`](#ArrayList)
  - [`ArrayList()`](#ArrayList)
  - [`ArrayList()`](#ArrayList)
  - [`ArrayList()`](#ArrayList)
  - [`append()`](#append)
  - [`clear()`](#clear)
  - [`copy()`](#copy)
  - [`empty()`](#empty)
  - [`equals()`](#equals)
  - [`exists()`](#exists)
  - [`filter()`](#filter)
  - [`length()`](#length)
  - [`remove()`](#remove)
  - [`remove()`](#remove)
  - [`hasIndex()`](#hasIndex)
  - [`contains()`](#contains)
  - [`get()`](#get)
  - [`indexOf()`](#indexOf)
  - [`lastIndexOf()`](#lastIndexOf)
  - [`slice()`](#slice)
  - [`subSequence()`](#subSequence)
  - [`first()`](#first)
  - [`last()`](#last)
  - [`reversed()`](#reversed)
  - [`sorted()`](#sorted)
  - [`map()`](#map)
  - [`flatMap()`](#flatMap)
  - [`forEach()`](#forEach)
  - [`zip()`](#zip)
  - [`add()`](#add)
  - [`addAll()`](#addAll)
  - [`insert()`](#insert)
  - [`set()`](#set)
  - [`removeAt()`](#removeAt)
  - [`removeFirst()`](#removeFirst)
  - [`removeLast()`](#removeLast)
  - [`removeElement()`](#removeElement)
  - [`sort()`](#sort)
  - [`reverse()`](#reverse)
  - [`swap()`](#swap)
  - [`fill()`](#fill)
  - [`resize()`](#resize)
  - [`iterator()`](#iterator)
  - [`toString()`](#toString)
  - [`any()`](#any)
  - [`all()`](#all)
  - [`noneMatch()`](#noneMatch)
  - [`count()`](#count)
  - [`reduce()`](#reduce)
  - [`find()`](#find)
  - [`findIndex()`](#findIndex)
  - [`take()`](#take)
  - [`drop()`](#drop)
  - [`takeWhile()`](#takeWhile)
  - [`dropWhile()`](#dropWhile)
  - [`distinct()`](#distinct)
  - [`size()`](#size)
  - [`isEmpty()`](#isEmpty)
  - [`isNotEmpty()`](#isNotEmpty)
  - [`firstOrNone()`](#firstOrNone)
  - [`lastOrNone()`](#lastOrNone)
  - [`chunked()`](#chunked)
  - [`partition()`](#partition)
  - [`enumerate()`](#enumerate)
  - [`destroy()`](#destroy)
- [class `ArrayListIterator`](#class-arraylistiterator)
  - [`ArrayListIterator()`](#ArrayListIterator)
  - [`has()`](#has)
  - [`next()`](#next)

## Imports

- `uranite.collection.collection`
  - `Collection`
- `uranite.collection.mutable-sequence`
  - `MutableSequence`
- `uranite.collection.pair`
  - `Pair`
- `uranite.collection.sequence`
  - `Sequence`
- `uranite.errors.lookup`
  - `IndexError`
- `uranite.errors.state`
  - `StateError`
- `uranite.errors.value`
  - `ValueError`
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

## class `ArrayList`<E>

**Implements**: `MutableSequence<E>`

Resizable array-backed list with amortized O(1) append.

Elements are stored contiguously in a Memory buffer that doubles in capacity when full. Supports indexed access, insertion, removal, sorting, filtering, and functional transformations such as map and forEach.

### Fields

| Name | Type | Access |
|------|------|--------|
| `data` | `Memory<E>` | private |
| `count` | `Int` | private |
| `capacity` | `Int` | private |

### Methods

#### `function ArrayList( self ) -> Void`

Create an empty list with a default initial capacity of 16 elements.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function ArrayList( self, Int initialCapacity ) -> Void`

Create an empty list with the specified initial capacity.

**Parameters**:

- `initialCapacity` (`Int`)
- `The number of element slots to pre-allocate.`

**Complexity**:
- Time: `O(1)`
- Space: `O(n)`

#### `function ArrayList( self, Sequence<E> elements ) -> Void`

Create a list populated with all elements from the given sequence.

**Parameters**:

- `elements` (`Sequence<E>`)
- `The sequence of elements to copy into this list.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function ArrayList( self, Int initialCapacity, Sequence<E> elements ) -> Void`

Create a list with the specified capacity, populated from the given sequence.

**Parameters**:

- `initialCapacity` (`Int`)
- `The number of element slots to pre-allocate.`
- `elements` (`Sequence<E>`)
- `The sequence of elements to copy into this list.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function append( self, E element ) -> ArrayList<E>`

Append an element to the end of the list and return self for method chaining.

**Parameters**:

- `element` (`E`)
- `The element to append.`

**Returns**: — ArrayList<E>:
This list instance, allowing chained calls.

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1) amortized`

#### `function clear( self ) -> Void`

Remove all elements from the list by resetting the count to zero. 

#### `property copy( self ) -> ArrayList<E>`

`property` 

Return a shallow copy of this list.

**Returns**: — ArrayList<E>:
A new list containing the same elements in the same order.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property empty( self ) -> Boolean`

`property` 

Check whether the list contains no elements.

**Returns**: `Boolean` — True if the list has zero elements, False otherwise.

#### `function equals( self, Object object ) -> Boolean`

Compare this list with another object for element-wise equality.

**Parameters**:

- `object` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is an ArrayList of the same length with
equal elements at every index, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function exists( self, E element ) -> Boolean`

Check whether the given element exists in the list via linear scan.

**Parameters**:

- `element` (`E`)
- `The element to search for.`

**Returns**: — Boolean:
True if the element is found, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function filter( self, <type> callback ) -> ArrayList<E>`

Return a new list containing only elements that match the predicate.

**Parameters**:

- `callback` (`Callable<Boolean,<E>>`)
- `A predicate function that returns True for elements to include.`

**Returns**: — ArrayList<E>:
A new list with only the matching elements.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property length( self ) -> Int`

`property` 

Return the number of elements currently in the list. 

#### `function remove( self, E element ) -> Boolean`

Remove the first occurrence of the given element from the list.

**Parameters**:

- `element` (`E`)
- `The element to remove.`

**Returns**: `Boolean` — True if the element was found and removed, False otherwise.

#### `function remove( self, Int index ) -> Void`

Remove the element at the given index without returning it.

**Parameters**:

- `index` (`Int`)
- `The zero-based index of the element to remove.`

#### `function hasIndex( self, Int index ) -> Boolean`

Check whether the given index is within the valid range of this list.

**Parameters**:

- `index` (`Int`)
- `The index to check.`

**Returns**: — Boolean:
True if the index is valid (0 <= index < length), False otherwise.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function contains( self, E element ) -> Boolean`

Check whether the given element exists in the list via linear scan.

**Parameters**:

- `element` (`E`)
- `The element to search for.`

**Returns**: — Boolean:
True if the element is found, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function get( self, Int index ) -> E`

Retrieve the element at the given index.

**Parameters**:

- `index` (`Int`)
- `The zero-based index of the element to retrieve.`

**Returns**: `E` — The element at the specified index.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If index is negative or greater than or equal to the list size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function indexOf( self, E element ) -> Int`

Return the index of the first occurrence of the element, or -1 if absent.

**Parameters**:

- `element` (`E`)
- `The element to search for.`

**Returns**: — Int:
The zero-based index, or -1 if the element is not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function lastIndexOf( self, E element ) -> Int`

Return the index of the last occurrence of the element, or -1 if absent.

**Parameters**:

- `element` (`E`)
- `The element to search for.`

**Returns**: — Int:
The zero-based index of the last occurrence, or -1 if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function slice( self, Int start, Int end ) -> ArrayList<E>`

Return a new list containing elements from the start index up to but not including the end index.

**Parameters**:

- `start` (`Int`)
- `The inclusive start index.`
- `end` (`Int`)
- `The exclusive end index.`

**Returns**: — ArrayList<E>:
A new list with the sliced elements.

**Complexity**:
- Time: `O(k) where k = end - start`
- Space: `O(k)`

#### `function subSequence( self, Int start, Int end ) -> ArrayList<E>`

Return a new sub-list between the start and end indices.

**Parameters**:

- `start` (`Int`)
- `The inclusive start index.`
- `end` (`Int`)
- `The exclusive end index.`

**Returns**: `ArrayList<E>` — A new list containing the elements in the specified range.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If start or end is out of bounds, or start > end.

**Complexity**:
- Time: `O(K) where K = end - start`
- Space: `O(K) where K = end - start`

#### `property first( self ) -> E`

`property` 

Return the first element of the list.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If the list is empty.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property last( self ) -> E`

`property` 

Return the last element of the list.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If the list is empty.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property reversed( self ) -> ArrayList<E>`

`property` 

Return a new list with the elements in reverse order.

**Returns**: — ArrayList<E>:
A new list containing this list's elements reversed.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function sorted( self, <type> comparator ) -> ArrayList<E>`

Return a new sorted list using the provided comparator function.

**Parameters**:

- `comparator` (`Callable<Int,<E,E>>`)
- `A comparison function returning negative, zero, or positive Int.`

**Returns**: — ArrayList<E>:
A new list with elements sorted according to the comparator.

**Complexity**:
- Time: `O(n^2)`
- Space: `O(n)`

#### `function map( self, <type> transform ) -> ArrayList<E>`

Return a new list with the transform function applied to each element.

**Parameters**:

- `transform` (`Callable<E,<E>>`)
- `The transformation function to apply to each element.`

**Returns**: — ArrayList<E>:
A new list containing the transformed elements.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function flatMap( self, <type> transform ) -> ArrayList<E>`

Apply the transform to each element, then flatten the resulting lists into a single list.

**Parameters**:

- `transform` (`Callable<ArrayList<E>, <E>>`)
- `Function that maps each element to a list of elements.`

**Returns**: — ArrayList<E>:
New list containing all elements from all transformed sublists.

**Complexity**:
- Time: `O(n * m) where m is average sublist size`
- Space: `O(n * m)`

#### `function forEach( self, <type> action ) -> Void`

Execute an action for each element in the list.

**Parameters**:

- `action` (`Callable<Void,<E>>`)
- `The action to execute with each element.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function zip( self, ArrayList<E> other ) -> ArrayList<E>`

Interleave elements from this list and another list, alternating between them up to the length of the shorter list.

**Parameters**:

- `other` (`ArrayList<E>`)
- `The other list to interleave with.`

**Returns**: — ArrayList<E>:
A new list with interleaved elements.

**Complexity**:
- Time: `O(min(n, m))`
- Space: `O(min(n, m))`

#### `function add( self, E element ) -> Void`

Append an element to the end of the list, growing the internal buffer if the capacity has been reached.

**Parameters**:

- `element` (`E`)
- `The element to add.`

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1) amortized`

#### `function addAll( self, ArrayList<E> elements ) -> Void`

Append all elements from another list to the end of this list.

**Parameters**:

- `elements` (`ArrayList<E>`)
- `The list of elements to append.`

**Complexity**:
- Time: `O(m) where m is elements.length`
- Space: `O(1) amortized`

#### `function insert( self, Int index, E element ) -> Void`

Insert an element at the given index, shifting all subsequent elements one position to the right.

**Parameters**:

- `index` (`Int`)
- `The zero-based index at which to insert. Must be in [0, size].`
- `element` (`E`)
- `The element to insert.`

**Raises**:

- `IndexError` → `LookupError` → `Error` — If index is negative or greater than the list size.

**Complexity**:
- Time: `O(N) where N is the number of elements after the insertion point`
- Space: `O(1) amortized, O(N) on grow`

#### `function set( self, Int index, E element ) -> Void`

Replace the element at the given index with a new element.

**Parameters**:

- `index` (`Int`)
- `The zero-based index of the element to replace.`
- `element` (`E`)
- `The new element to store at the index.`

**Raises**:

- `IndexError` → `LookupError` → `Error` — If index is negative or greater than or equal to the list size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function removeAt( self, Int index ) -> E`

Remove and return the element at the given index, shifting all subsequent elements one position to the left.

**Parameters**:

- `index` (`Int`)
- `The zero-based index of the element to remove.`

**Returns**: `E` — The removed element.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If index is negative or greater than or equal to the list size.

**Complexity**:
- Time: `O(N) where N is the number of elements after the removal point`
- Space: `O(1)`

#### `function removeFirst( self ) -> E`

Remove and return the first element of the list.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If the list is empty.

**Complexity**:
- Time: `O(N) where N is the number of elements (shifts all)`
- Space: `O(1)`

#### `function removeLast( self ) -> E`

Remove and return the last element of the list.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If the list is empty.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function removeElement( self, E element ) -> Boolean`

Remove the first occurrence of the given element from the list.

**Parameters**:

- `element` (`E`)
- `The element to remove.`

**Returns**: — Boolean:
True if the element was found and removed, False otherwise.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function sort( self ) -> Void`

Sort elements in-place using insertion sort.

**Complexity**:
- Time: `O(n^2)`
- Space: `O(1)`

#### `function reverse( self ) -> Void`

Reverse the order of elements in-place by swapping from both ends inward.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function swap( self, Int indexA, Int indexB ) -> Void`

Swap the elements at two indices.

**Parameters**:

- `indexA` (`Int`)
- `The index of the first element.`
- `indexB` (`Int`)
- `The index of the second element.`

**Raises**:

- `IndexError` → `LookupError` → `Error` — If either index is negative or greater than or equal to the list size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fill( self, E element ) -> Void`

Set all elements in the list to the given value.

**Parameters**:

- `element` (`E`)
- `The value to fill every slot with.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function resize( self, Int newSize ) -> Void`

Change the logical size of the list.

**Parameters**:

- `newSize` (`Int`)
- `The new element count.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property iterator( self ) -> Iterator<E>`

`property` 

Return an iterator over the elements of this list.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function grow( self ) -> Void`

Double the internal capacity and copy all elements to the new buffer.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function toString( self ) -> String`

Return a string representation of the list in the format [elem1, elem2, ...].

**Returns**: — String:
The formatted string representation.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function any( self, <type> predicate ) -> Boolean`

Test whether at least one element satisfies the predicate.

Short-circuits on the first match found.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns true for a matching element.`

**Returns**: — Boolean:
True if any element satisfies the predicate, false if none do.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function all( self, <type> predicate ) -> Boolean`

Test whether every element satisfies the predicate.

Short-circuits on the first non-matching element.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns true for a conforming element.`

**Returns**: — Boolean:
True if all elements satisfy the predicate, false otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function noneMatch( self, <type> predicate ) -> Boolean`

Test whether no element satisfies the predicate.

Equivalent to `not self.any( predicate )` but reads more clearly when asserting absence of a condition.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns true for a matching element.`

**Returns**: — Boolean:
True if no element satisfies the predicate.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function count( self, <type> predicate ) -> Int`

Count the number of elements satisfying the predicate.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns true for elements to count.`

**Returns**: — Int:
The number of matching elements.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function reduce( self, <type> accumulator ) -> E`

Reduce all elements to a single value by repeatedly applying the accumulator function left-to-right.

The first element is used as the initial accumulator value.

**Parameters**:

- `accumulator` (`Callable<E, <E, E>>`)
- `Binary function taking` (`accumulated, current`)
- `the new accumulated value.`

**Returns**: `E` — The final accumulated result.

**Raises**:

- `StateError` → `Error` — If the list is empty (no initial element to seed reduction).

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function find( self, <type> predicate ) -> ?E`

Find the first element satisfying the predicate.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns true for the desired element.`

**Returns**: — ?E:
The first matching element, or None if no match exists.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findIndex( self, <type> predicate ) -> Int`

Find the index of the first element satisfying the predicate.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns true for the desired element.`

**Returns**: — Int:
Index of the first match, or -1 if no match exists.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function take( self, Int amount ) -> ArrayList<E>`

Return a new list containing at most the first N elements.

If amount exceeds the list size, all elements are included.

**Parameters**:

- `amount` (`Int`)
- `Maximum number of elements to take from the front.`

**Returns**: `ArrayList<E>` — New list with up to amount elements.

**Raises**:

- `ValueError` → `Error` — If amount is negative.

**Complexity**:
- Time: `O(amount)`
- Space: `O(amount)`

#### `function drop( self, Int amount ) -> ArrayList<E>`

Return a new list with the first N elements removed.

If amount exceeds the list size, an empty list is returned.

**Parameters**:

- `amount` (`Int`)
- `Number of elements to skip from the front.`

**Returns**: `ArrayList<E>` — New list without the first amount elements.

**Raises**:

- `ValueError` → `Error` — If amount is negative.

**Complexity**:
- Time: `O(n - amount)`
- Space: `O(n - amount)`

#### `function takeWhile( self, <type> predicate ) -> ArrayList<E>`

Return a new list of leading elements that satisfy the predicate.

Stops at the first element that does not match.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns true for elements to include.`

**Returns**: — ArrayList<E>:
New list of the longest matching prefix.

**Complexity**:
- Time: `O(k) where k is the prefix length`
- Space: `O(k)`

#### `function dropWhile( self, <type> predicate ) -> ArrayList<E>`

Return a new list with leading elements removed while the predicate holds.

Starts including elements from the first non-matching element onward.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `Function that returns true for elements to skip.`

**Returns**: — ArrayList<E>:
New list starting from the first non-matching element.

**Complexity**:
- Time: `O(n)`
- Space: `O(n - k) where k is the dropped prefix length`

#### `function distinct( self ) -> ArrayList<E>`

Return a new list with duplicate elements removed.

Preserves the order of first occurrence. Uses linear scan for duplicate detection (suitable for small to medium lists).

**Returns**: — ArrayList<E>:
New list containing only unique elements.

**Complexity**:
- Time: `O(n^2)`
- Space: `O(n)`

#### `property size( self ) -> Int`

`property` 

Return the number of elements in the list.

Alias for the length property to match common collection conventions.

**Returns**: — Int:
The element count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property isEmpty( self ) -> Boolean`

`property` 

Test whether the list contains no elements.

**Returns**: — Boolean:
True if the list has zero elements.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property isNotEmpty( self ) -> Boolean`

`property` 

Test whether the list contains at least one element.

**Returns**: — Boolean:
True if the list has one or more elements.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function firstOrNone( self ) -> ?E`

Return the first element, or None if the list is empty.

**Returns**: — ?E:
The first element, or None.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function lastOrNone( self ) -> ?E`

Return the last element, or None if the list is empty.

**Returns**: — ?E:
The last element, or None.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function chunked( self, Int chunkSize ) -> ArrayList<ArrayList<E>>`

Split the list into sublists of the given size. The last chunk may contain fewer elements if the list size is not evenly divisible.

**Parameters**:

- `chunkSize` (`Int`)
- `The maximum number of elements per chunk.`

**Returns**: `ArrayList<ArrayList<E>>` — A list of chunk sublists.

**Raises**:

- `ValueError` → `Error` — If chunkSize is less than or equal to zero.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function partition( self, <type> predicate ) -> Pair<ArrayList<E>, ArrayList<E>>`

Split the list into two sublists: elements matching the predicate and elements not matching it.

**Parameters**:

- `predicate` (`Callable<Boolean, <E>>`)
- `The test function applied to each element.`

**Returns**: — Pair<ArrayList<E>, ArrayList<E>>:
A pair where key holds matching elements and value holds
non-matching elements.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function enumerate( self ) -> ArrayList<Pair<Int, E>>`

Pair each element with its zero-based index.

**Returns**: — ArrayList<Pair<Int, E>>:
A list of index-element pairs.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function destroy( self ) -> Void`

Release the heap-allocated data buffer held by this list.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ArrayListIterator`<E>

**Implements**: `Iterator<E>`

Sequential iterator over ArrayList elements.

Maintains a position cursor that advances through the list from the first element to the last.

### Fields

| Name | Type | Access |
|------|------|--------|
| `source` | `ArrayList<E>` | private |
| `position` | `Int` | private |

### Methods

#### `function ArrayListIterator( self, ArrayList<E> source ) -> Void`

Create an iterator starting at the beginning of the given list.

**Parameters**:

- `source` (`ArrayList<E>`)
- `The list to iterate over.`

#### `property has( self ) -> Boolean`

`property` 

Check whether more elements remain to be iterated.

**Returns**: `Boolean` — True if the iterator has not yet reached the end of the list.

#### `property next( self ) -> E`

`property` 

Return the current element and advance the iterator position by one.

**Returns**: — E:
The element at the current position before advancing.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

