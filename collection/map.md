# uranite.collection.map

## Table of Contents

- [Imports](#imports)
- [interface `Map`](#interface-map)
  - [`getOrDefault()`](#getOrDefault)
  - [`containsKey()`](#containsKey)
  - [`containsValue()`](#containsValue)
  - [`keys()`](#keys)
  - [`values()`](#values)
  - [`entries()`](#entries)
  - [`forEach()`](#forEach)

## Imports

- `uranite.collection.collection`
  - `Collection`
- `uranite.collection.pair`
  - `Pair`
- `uranite.collection.sequence`
  - `Sequence`
- `uranite.collection.set`
  - `Set`
- `uranite.iterators.iterable`
  - `Iterable`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.callable`
  - `Callable`
- `uranite.language.void`
  - `Void`
- `uranite.operators.indexable`
  - `Indexable`

## interface `Map`<K, V>

**Implements**: `Collection<V>`, `Indexable<K, V>`, `Iterable<Pair<K, V>>`

Key-value associative container mapping keys of type K to values of type V.

Extends Collection and Indexable to provide key-based lookup, containment checks, and views over keys, values, and entries.

### Methods

#### `function getOrDefault( K key, V defaultValue ) -> V`

Retrieve the value for the given key, or return the default value if absent. 

#### `function containsKey( K key ) -> Boolean`

Check whether the given key exists in the map. 

#### `function containsValue( V value ) -> Boolean`

Check whether the given value exists in the map. 

#### `property keys(  ) -> Set<K>`

`property` 

Return a set containing all keys in the map. 

#### `property values(  ) -> Sequence<V>`

`property` 

Return a sequence containing all values in the map. 

#### `property entries(  ) -> Set<Pair<K, V>>`

`property` 

Return a set of all key-value pairs in the map. 

#### `function forEach( <type> action ) -> Void`

Execute the given action for each key-value pair in the map. 

