# uranite.iterators.iterable

## Table of Contents

- [Imports](#imports)
- [interface `Iterable`](#interface-iterable)
  - [`iterator()`](#iterator)

## Imports

- `uranite.iterators.iterator`
  - `Iterator`
- `uranite.iterators.traversable`
  - `Traversable`

## interface `Iterable`<T>

**Implements**: `Traversable`

Generic interface for types that can produce an Iterator over their contained elements of type T. Extending Traversable, this interface provides the concrete mechanism for sequential element access through the iterator property. Collections such as ArrayList, HashSet, and HashMap implement this interface to enable for-each loop traversal and other iteration patterns. Each call to the iterator property should return a fresh Iterator starting from the first element, allowing multiple independent traversals over the same collection.

### Methods

#### `property iterator(  ) -> Iterator<T>`

`property` 

Return a fresh Iterator instance positioned at the first element of this collection. Each invocation creates a new independent iterator, allowing multiple concurrent traversals over the same underlying data without interference. The returned iterator yields elements of type T in the collection's natural ordering.

**Returns**: `Iterator<T>` — A new iterator starting from the first element of this collection.

