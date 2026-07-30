# uranite.collection.kwargs

## Table of Contents

- [Imports](#imports)
- [class `Kwargs`](#class-kwargs)
  - [`Kwargs()`](#Kwargs)
  - [`set()`](#set)
  - [`remove()`](#remove)
  - [`get()`](#get)
  - [`getOrDefault()`](#getOrDefault)
  - [`contains()`](#contains)
  - [`containsKey()`](#containsKey)
  - [`containsValue()`](#containsValue)
  - [`keys()`](#keys)
  - [`values()`](#values)
  - [`entries()`](#entries)
  - [`forEach()`](#forEach)
  - [`append()`](#append)
  - [`append()`](#append)
  - [`clear()`](#clear)
  - [`copy()`](#copy)
  - [`empty()`](#empty)
  - [`equals()`](#equals)
  - [`exists()`](#exists)
  - [`exists()`](#exists)
  - [`filter()`](#filter)
  - [`length()`](#length)
  - [`isEmpty()`](#isEmpty)
  - [`remove()`](#remove)
  - [`remove()`](#remove)
  - [`iterator()`](#iterator)
  - [`size()`](#size)
  - [`isNotEmpty()`](#isNotEmpty)
  - [`destroy()`](#destroy)
- [class `KwargsIterator`](#class-kwargsiterator)
  - [`KwargsIterator()`](#KwargsIterator)
  - [`has()`](#has)
  - [`next()`](#next)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.collection.collection`
  - `Collection`
- `uranite.collection.hash-set`
  - `HashSet`
- `uranite.collection.immutable-mapping`
  - `ImmutableMapping`
- `uranite.collection.pair`
  - `Pair`
- `uranite.collection.sequence`
  - `Sequence`
- `uranite.collection.set`
  - `Set`
- `uranite.errors.lookup`
  - `KeyError`
- `uranite.iterators.iterator`
  - `Iterator`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.callable`
  - `Callable`
- `uranite.language.void`
  - `Void`
- `uranite.memory.memory`
  - `Memory`

## class `Kwargs`<T>

**Implements**: `ImmutableMapping<String, T>`

Immutable keyword argument container mapping String keys to values of type T.

Constructed by the compiler at call sites for keyword parameter declarations. Wraps parallel Memory buffers for keys and values with a fixed count. All mutation methods inherited from Collection are no-ops since the container is immutable after construction.

### Fields

| Name | Type | Access |
|------|------|--------|
| `keys` | `Memory<String>` | protect |
| `values` | `Memory<T>` | protect |
| `count` | `I64` | protect |

### Methods

#### `function Kwargs( self, Memory<String> keys, Memory<T> values, I64 count ) -> Void`

Create a new keyword argument container from parallel key and value buffers.

**Parameters**:

- `keys` (`Memory<String>`)
- `The buffer containing parameter names.`
- `values` (`Memory<T>`)
- `The buffer containing parameter values corresponding to each key.`
- `count` (`I64`)
- `The number of key-value pairs in the container.`

#### `function set( self, String key, T value ) -> Void`

No-op for immutable keyword arguments. 

#### `function remove( self, String key ) -> Void`

No-op for immutable keyword arguments. 

#### `function get( self, String key ) -> T`

Retrieve the value associated with the given key by scanning all entries.

**Parameters**:

- `key` (`String`)
- `The parameter name to look up.`

**Returns**: `T` — The value associated with the key.

**Raises**:

- `KeyError` → `LookupError` → `Error` — If the key is not present in the keyword arguments.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function getOrDefault( self, String key, T defaultValue ) -> T`

Retrieve the value for the given key, or return the default if absent.

**Parameters**:

- `key` (`String`)
- `The parameter name to look up.`
- `defaultValue` (`T`)
- `The value to return if the key is not present.`

**Returns**: — T:
The associated value, or the default.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function contains( self, String key ) -> Boolean`

Check whether the given key exists in the keyword arguments.

**Parameters**:

- `key` (`String`)
- `The parameter name to search for.`

**Returns**: — Boolean:
True if the key is present, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function containsKey( self, String key ) -> Boolean`

Check whether the given key exists in the keyword arguments.

**Parameters**:

- `key` (`String`)
- `The parameter name to search for.`

**Returns**: `Boolean` — True if the key is present, False otherwise.

#### `function containsValue( self, T value ) -> Boolean`

Check whether the given value exists in the keyword arguments via linear scan of all values.

**Parameters**:

- `value` (`T`)
- `The value to search for.`

**Returns**: — Boolean:
True if the value is found, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property keys( self ) -> Set<String>`

`property` 

Return a set containing all parameter names in the keyword arguments.

**Returns**: — Set<String>:
A HashSet of all keys.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property values( self ) -> Sequence<T>`

`property` 

Return a sequence containing all parameter values in the keyword arguments.

**Returns**: — Sequence<T>:
An ArrayList of all values.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property entries( self ) -> Set<Pair<String, T>>`

`property` 

Return a set of all key-value pairs in the keyword arguments.

**Returns**: — Set<Pair<String, T>>:
A HashSet of Pair objects representing each entry.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function forEach( self, <type> action ) -> Void`

Execute an action for each key-value pair in the keyword arguments.

**Parameters**:

- `action` (`Callable<Void,<String,T>>`)
- `The action to execute, receiving the key and value as arguments.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function append( self, T element ) -> Collection<T>`

No-op for immutable keyword arguments; returns self unchanged.

**Parameters**:

- `element` (`T`)
- `The element that would be appended` (`ignored`)

**Returns**: `Collection<T>` — This container unchanged.

#### `function append( self, [T] element[] ) -> Collection<T>`

No-op for immutable keyword arguments; returns self unchanged.

**Parameters**:

- `element` (`T[]`)
- `The elements that would be appended` (`ignored`)

**Returns**: `Collection<T>` — This container unchanged.

#### `function clear( self ) -> Void`

No-op for immutable keyword arguments. 

#### `property copy( self ) -> Collection<T>`

`property` 

Return a shallow copy of this keyword argument container.

**Returns**: — Collection<T>:
A new Kwargs with copies of the key and value buffers.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property empty( self ) -> Boolean`

`property` 

Check whether the keyword arguments contain no entries.

**Returns**: `Boolean` — True if the container has zero entries, False otherwise.

#### `function equals( self, Object object ) -> Boolean`

Compare this keyword argument container with another object for structural equality of all key-value pairs.

**Parameters**:

- `object` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is a Kwargs with identical entries.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function exists( self, T element ) -> Boolean`

Check whether the given value exists among the keyword argument values.

**Parameters**:

- `element` (`T`)
- `The value to search for.`

**Returns**: `Boolean` — True if the value is found, False otherwise.

#### `function exists( self, [T] element[] ) -> Boolean`

Check whether all of the given values exist among the keyword argument values.

**Parameters**:

- `element` (`T[]`)
- `The values to search for.`

**Returns**: — Boolean:
True if every element is found, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function filter( self, <type> callback ) -> Collection<T>`

Return a new collection containing only entries whose values match the predicate. Since Kwargs is immutable, the result is an ArrayList of matching values.

**Parameters**:

- `callback` (`Callable<Boolean,<T>>`)
- `A predicate function that returns True for values to include.`

**Returns**: — Collection<T>:
An ArrayList containing only the matching values.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property length( self ) -> Int`

`property` 

Return the number of key-value pairs in the keyword arguments.

**Returns**: `Int` — The entry count.

#### `function isEmpty( self ) -> Boolean`

Check whether the keyword arguments contain no entries.

**Returns**: `Boolean` — True if the container has zero entries, False otherwise.

#### `function remove( self, T element ) -> Boolean`

No-op for immutable keyword arguments; always returns False.

**Parameters**:

- `element` (`T`)
- `The element that would be removed` (`ignored`)

**Returns**: `Boolean` — Always False since the container is immutable.

#### `function remove( self, [T] element[] ) -> Boolean`

No-op for immutable keyword arguments; always returns False.

**Parameters**:

- `element` (`T[]`)
- `The elements that would be removed` (`ignored`)

**Returns**: `Boolean` — Always False since the container is immutable.

#### `property iterator( self ) -> Iterator<Pair<String, T>>`

`property` 

Return a fresh iterator over the key-value pairs in the keyword arguments.

**Returns**: — Iterator<Pair<String, T>>:
A new KwargsIterator starting from the first entry.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property size( self ) -> Int`

`property` 

Return the number of key-value pairs in the keyword arguments.

Alias for the length property to match common collection conventions.

**Returns**: — Int:
The entry count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property isNotEmpty( self ) -> Boolean`

`property` 

Test whether the keyword arguments contain at least one entry.

**Returns**: — Boolean:
True if the container has one or more entries.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release the heap-allocated key and value buffers held by this container.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `KwargsIterator`<T>

**Implements**: `Iterator<Pair<String, T>>`

Iterator that traverses a Kwargs container, yielding key-value pairs as Pair<String, T> instances in insertion order.

### Fields

| Name | Type | Access |
|------|------|--------|
| `keyData` | `Memory<String>` | protect |
| `valueData` | `Memory<T>` | protect |
| `count` | `I64` | protect |
| `position` | `I64` | protect |

### Methods

#### `function KwargsIterator( self, Memory<String> keyData, Memory<T> valueData, I64 count ) -> Void`

Create an iterator over the given key and value buffers.

**Parameters**:

- `keyData`
- `valueData`
- `count`

#### `property has( self ) -> Boolean`

`property` 

Check whether more entries remain to be iterated.

**Returns**: `Boolean` — True if the iterator has not yet reached the end.

#### `property next( self ) -> Pair<String, T>`

`property` 

Return the current key-value pair and advance to the next position.

**Returns**: — Pair<String, T>:
The key-value pair at the current position before advancing.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

