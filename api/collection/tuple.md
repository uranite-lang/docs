# uranite.collection.tuple

## Table of Contents

- [Imports](#imports)
- [class `Tuple`](#class-tuple)
  - [`Tuple()`](#Tuple)
  - [`component()`](#component)
  - [`set()`](#set)
  - [`get()`](#get)
  - [`length()`](#length)
  - [`size()`](#size)
  - [`concatenate()`](#concatenate)
  - [`equals()`](#equals)
  - [`hashCode()`](#hashCode)
  - [`toString()`](#toString)
  - [`iterator()`](#iterator)
- [class `TupleIterator`](#class-tupleiterator)
  - [`TupleIterator()`](#TupleIterator)
  - [`has()`](#has)
  - [`next()`](#next)

## Imports

- `uranite.iterators.iterable`
  - `Iterable`
- `uranite.iterators.iterator`
  - `Iterator`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.string`
  - `String`
- `uranite.memory`
  - `Memory`

## class `Tuple`<E>

**Implements**: `Iterable<E>`

Fixed-size immutable ordered sequence of elements.

A Tuple holds a fixed number of elements determined at construction time. Elements are stored contiguously in a Memory buffer. Unlike mutable collections, tuples cannot grow or shrink after creation, making them suitable for lightweight composite values and multi-variable destructuring.

### Fields

| Name | Type | Access |
|------|------|--------|
| `data` | `Memory<E>` | private |
| `count` | `Int` | private |

### Methods

#### `function Tuple( self, Int size ) -> Void`

Create a tuple with the specified number of element slots.

**Parameters**:

- `size` (`Int`)
- `The fixed number of elements this tuple will hold.`

**Complexity**:
- Time: `O(1)`
- Space: `O(n)`

#### `function component( self, Int index ) -> E`

Access an element by its positional index within the tuple.

**Parameters**:

- `index` (`Int`)
- `The zero-based position of the element to retrieve.`

**Returns**: — E:
The element at the specified position.

**Complexity**:
- Time: `O(1)`

#### `function set( self, Int index, E value ) -> Void`

Store an element at the given index. Used during construction from literal desugaring to populate tuple slots.

**Parameters**:

- `index` (`Int`)
- `The zero-based position to store the element.`
- `value` (`E`)
- `The element to store.`

**Complexity**:
- Time: `O(1)`

#### `function get( self, Int index ) -> E`

Retrieve the element at the given index.

**Parameters**:

- `index` (`Int`)
- `The zero-based position of the element.`

**Returns**: — E:
The element at the specified position.

**Complexity**:
- Time: `O(1)`

#### `property length( self ) -> Int`

`property` 

Return the number of elements in this tuple.

**Returns**: — Int:
The fixed element count.

**Complexity**:
- Time: `O(1)`

#### `function size( self ) -> Int`

Return the number of elements in this tuple.

**Returns**: — Int:
The fixed element count.

**Complexity**:
- Time: `O(1)`

#### `function concatenate( self, Tuple<E> other ) -> Tuple<E>`

Join this tuple with another tuple, producing a new tuple containing all elements from both.

**Parameters**:

- `other` (`Tuple<E>`)
- `The tuple to concatenate with this one.`

**Returns**: — Tuple<E>:
A new tuple with elements from both tuples.

**Complexity**:
- Time: `O(n + m)`
- Space: `O(n + m)`

#### `function equals( self, Object other ) -> Boolean`

Compare this tuple with another object for structural equality.

Two tuples are equal if they have the same length and each element at every position is equal.

**Parameters**:

- `other` (`Object`)
- `The object to compare against.`

**Returns**: `Boolean` — True if the other object is a tuple with equal elements.

#### `function hashCode( self ) -> Int`

Compute a hash code from all elements of this tuple.

**Returns**: `Int` — A hash code value derived from the elements.

#### `function toString( self ) -> String`

Return a string representation of this tuple.

**Returns**: `String` — A human-readable string in the format "(e1, e2, ...)".

#### `property iterator( self ) -> Iterator<E>`

`property` 

Return an iterator over the elements of this tuple.

**Returns**: — Iterator<E>:
A new iterator starting at the first element.

**Complexity**:
- Time: `O(1)`

## class `TupleIterator`<E>

**Implements**: `Iterator<E>`

Sequential iterator over Tuple elements.

Maintains a position cursor that advances through the tuple from the first element to the last.

### Fields

| Name | Type | Access |
|------|------|--------|
| `source` | `Tuple<E>` | private |
| `position` | `Int` | private |

### Methods

#### `function TupleIterator( self, Tuple<E> source ) -> Void`

Create an iterator starting at the beginning of the given tuple.

**Parameters**:

- `source` (`Tuple<E>`)
- `The tuple to iterate over.`

#### `property has( self ) -> Boolean`

`property` 

Check whether more elements remain to be iterated.

**Returns**: `Boolean` — True if the iterator has not reached the end of the tuple.

#### `property next( self ) -> E`

`property` 

Return the current element and advance the iterator position.

**Returns**: — E:
The element at the current position before advancing.

**Complexity**:
- Time: `O(1)`

