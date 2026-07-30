# uranite.collection.args

## Table of Contents

- [Imports](#imports)
- [class `Args`](#class-args)
  - [`Args()`](#Args)
  - [`has()`](#has)
  - [`next()`](#next)
  - [`get()`](#get)
  - [`set()`](#set)
  - [`remove()`](#remove)
  - [`contains()`](#contains)
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
  - [`isEmpty()`](#isEmpty)
  - [`remove()`](#remove)
  - [`remove()`](#remove)
  - [`iterator()`](#iterator)
  - [`indexOf()`](#indexOf)
  - [`lastIndexOf()`](#lastIndexOf)
  - [`slice()`](#slice)
  - [`subSequence()`](#subSequence)
  - [`first()`](#first)
  - [`last()`](#last)
  - [`reversed()`](#reversed)
  - [`sorted()`](#sorted)
  - [`map()`](#map)
  - [`zip()`](#zip)
  - [`with()`](#with)
  - [`appended()`](#appended)
  - [`prepended()`](#prepended)
  - [`concatenated()`](#concatenated)
  - [`filtered()`](#filtered)
  - [`dropped()`](#dropped)
  - [`taken()`](#taken)
  - [`destroy()`](#destroy)

## Imports

- `uranite.collection.collection`
  - `Collection`
- `uranite.collection.immutable-sequence`
  - `ImmutableSequence`
- `uranite.collection.sequence`
  - `Sequence`
- `uranite.iterators.iterator`
  - `Iterator`
- `uranite.language.callable`
  - `Callable`
- `uranite.language.void`
  - `Void`
- `uranite.memory.memory`
  - `Memory`

## class `Args`<T>

**Implements**: `ImmutableSequence<T>`, `Iterator<T>`

A read-only container for variadic function parameters. Wraps a contiguous Memory<T> buffer with an element count. Instances are constructed automatically by the compiler at call sites when a function accepts a variadic parameter (Type name[]).

### Fields

| Name | Type | Access |
|------|------|--------|
| `data` | `Memory<T>` | protect |
| `count` | `I64` | protect |
| `position` | `I64` | protect |

### Methods

#### `function Args( self, Memory<T> data, I64 count ) -> Void`

Create a new Args wrapping a memory buffer with the given element count. The iterator position is initialized to zero.

**Parameters**:

- `data` (`Memory<T>`)
- `The memory buffer containing the argument values.`
- `count` (`I64`)
- `The number of elements in the buffer.`

#### `property has( self ) -> Boolean`

`property` 

Check whether the iterator has more elements to yield. 

#### `property next( self ) -> T`

`property` 

Return the next element and advance the iterator position. 

#### `function get( self, I64 index ) -> T`

Retrieve the element at the given index. 

#### `function set( self, I64 index, T value ) -> Void`

No-op for immutable variadic arguments. 

#### `function remove( self, I64 index ) -> Void`

No-op for immutable variadic arguments. 

#### `function contains( self, T element ) -> Boolean`

Check whether the argument list contains the given element. 

#### `function append( self, T element ) -> Collection<T>`

No-op for immutable variadic arguments; returns self unchanged. 

#### `function append( self, [T] element[] ) -> Collection<T>`

No-op for immutable variadic arguments; returns self unchanged. 

#### `function clear( self ) -> Void`

No-op for immutable variadic arguments. 

#### `property copy( self ) -> Collection<T>`

`property` 

Return a shallow copy of this argument list. 

#### `property empty( self ) -> Boolean`

`property` 

Check whether the argument list contains no elements. 

#### `function equals( self, Object object ) -> Boolean`

Compare this argument list with another object for structural equality. 

#### `function exists( self, T element ) -> Boolean`

Check whether the given element exists in the argument list. 

#### `function exists( self, [T] element[] ) -> Boolean`

Check whether all given elements exist in the argument list. 

#### `function filter( self, <type> callback ) -> Collection<T>`

Return a new collection containing only elements that match the predicate. 

#### `function forEach( self, <type> action ) -> Void`

Execute an action for each element in the argument list. 

#### `property length( self ) -> I64`

`property` 

Return the number of elements in this argument list. 

#### `function isEmpty( self ) -> Boolean`

Check whether this argument list contains no elements. 

#### `function remove( self, T element ) -> Boolean`

No-op for immutable variadic arguments; always returns False. 

#### `function remove( self, [T] element[] ) -> Boolean`

No-op for immutable variadic arguments; always returns False. 

#### `property iterator( self ) -> Iterator<T>`

`property` 

Return a fresh iterator over the elements in this argument list. 

#### `function indexOf( self, T element ) -> I64`

Find the index of the first occurrence of the given element, or -1 if absent. 

#### `function lastIndexOf( self, T element ) -> I64`

Find the index of the last occurrence of the given element, or -1 if absent. 

#### `function slice( self, I64 start, I64 end ) -> Sequence<T>`

Return a new sequence containing elements from start up to but not including end. 

#### `function subSequence( self, I64 start, I64 end ) -> Sequence<T>`

Return a sub-sequence containing elements between start and end indices. 

#### `property first( self ) -> T`

`property` 

Return the first element in the argument list. 

#### `property last( self ) -> T`

`property` 

Return the last element in the argument list. 

#### `property reversed( self ) -> Sequence<T>`

`property` 

Return a new sequence with the elements in reverse order. 

#### `function sorted( self, <type> comparator ) -> Sequence<T>`

Return a new sorted sequence using the provided comparator. 

#### `function map( self, <type> transform ) -> Sequence<T>`

Return a new sequence with the transform applied to each element. 

#### `function zip( self, Sequence<T> other ) -> Sequence<T>`

Interleave elements from this and the other sequence. 

#### `function with( self, I64 index, T element ) -> ImmutableSequence<T>`

Return a new Args with the element at the given index replaced. 

#### `function appended( self, T element ) -> ImmutableSequence<T>`

Return a new Args with the given element appended to the end. 

#### `function prepended( self, T element ) -> ImmutableSequence<T>`

Return a new Args with the given element prepended to the front. 

#### `function concatenated( self, Sequence<T> other ) -> ImmutableSequence<T>`

Return a new Args with the other sequence concatenated to the end. 

#### `function filtered( self, <type> predicate ) -> ImmutableSequence<T>`

Return a new Args containing only elements that match the predicate. 

#### `function dropped( self, I64 dropCount ) -> ImmutableSequence<T>`

Return a new Args with the first dropCount elements removed. 

#### `function taken( self, I64 takeCount ) -> ImmutableSequence<T>`

Return a new Args containing only the first takeCount elements. 

#### `function destroy( self ) -> Void`

Release the heap-allocated data buffer held by this variadic container. 

