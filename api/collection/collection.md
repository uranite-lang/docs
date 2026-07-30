# uranite.collection.collection

## Table of Contents

- [Imports](#imports)
- [interface `Collection`](#interface-collection)
  - [`append()`](#append)
  - [`append()`](#append)
  - [`clear()`](#clear)
  - [`copy()`](#copy)
  - [`empty()`](#empty)
  - [`equals()`](#equals)
  - [`exists()`](#exists)
  - [`exists()`](#exists)
  - [`filter()`](#filter)
  - [`forEach()`](#forEach)
  - [`length()`](#length)
  - [`remove()`](#remove)
  - [`remove()`](#remove)

## Imports

- `uranite.iterators.iterable`
  - `Iterable`
- `uranite.iterators.iterator`
  - `Iterator`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.callable`
  - `Callable`
- `uranite.language.void`
  - `Void`

## interface `Collection`<T>

**Implements**: `Iterable<T>`

Base interface for all collections that hold elements of type T.

Provides the common contract for appending, removing, filtering, iterating, and querying elements that all collection types must support.

### Methods

#### `function append( self, T element ) -> Collection<T>`

Append a single element to the collection and return self for chaining. 

#### `function append( self, [T] element[] ) -> Collection<T>`

Append multiple elements to the collection and return self for chaining. 

#### `function clear( self ) -> Void`

Remove all elements from the collection. 

#### `property copy( self ) -> Collection<T>`

`property` 

Return a shallow copy of this collection. 

#### `property empty( self ) -> Boolean`

`property` 

Check whether the collection contains no elements. 

#### `function equals( self, Object object ) -> Boolean`

Compare this collection with another object for structural equality. 

#### `function exists( self, T element ) -> Boolean`

Check whether the given element exists in the collection. 

#### `function exists( self, [T] element[] ) -> Boolean`

Check whether all of the given elements exist in the collection. 

#### `function filter( self, <type> callback ) -> Collection<T>`

Return a new collection containing only elements that match the predicate. 

#### `function forEach( self, <type> action ) -> Void`

Execute an action for each element in the collection. 

#### `property length( self ) -> Int`

`property` 

Return the number of elements in the collection. 

#### `function remove( self, T element ) -> Boolean`

Remove the given element from the collection, returning True if it was found. 

#### `function remove( self, [T] element[] ) -> Boolean`

Remove multiple elements from the collection, returning True if any were found. 

