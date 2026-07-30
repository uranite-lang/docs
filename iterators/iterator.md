# uranite.iterators.iterator

## Table of Contents

- [Imports](#imports)
- [interface `Iterator`](#interface-iterator)
  - [`has()`](#has)
  - [`next()`](#next)

## Imports

- `uranite.language`
  - `Boolean`

## interface `Iterator`<T>

Stateful cursor interface that yields elements of type T one at a time from a collection or data source. An Iterator maintains an internal position that advances with each call to next, and provides the has property to check whether more elements remain before attempting to retrieve them. Once all elements have been consumed (has returns false), calling next produces undefined behavior. Iterators are single-use and forward-only; to traverse a collection again, obtain a new Iterator from the Iterable interface.

### Methods

#### `property has( self ) -> Boolean`

`property` 

Check whether this iterator has more elements remaining to yield. Returns true if a subsequent call to next will produce a valid element, or false if the iterator has been exhausted and all elements have been consumed. This should be checked before each call to next to avoid accessing past the end of the underlying data source.

**Returns**: `Boolean` — True if more elements remain, false if the iterator is exhausted.

#### `property next( self ) -> T`

`property` 

Return the current element and advance the iterator cursor to the next position. Implementations must raise StateError when called on an exhausted iterator (when has returns false). The returned element is of the generic type T matching the collection's element type.

**Returns**: `T` — The current element at the iterator's position before advancing.

**Raises**:

- `StateError` → `Error` — If the iterator has been exhausted and no elements remain.

