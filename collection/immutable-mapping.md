# uranite.collection.immutable-mapping

## Table of Contents

- [Imports](#imports)
- [interface `ImmutableMapping`](#interface-immutablemapping)
  - [`get()`](#get)
  - [`contains()`](#contains)
  - [`length()`](#length)
  - [`isEmpty()`](#isEmpty)

## Imports

- `uranite.collection.map`
  - `Map`
- `uranite.language.boolean`
  - `Boolean`

## interface `ImmutableMapping`<K, V>

**Implements**: `Map<K, V>`

### Methods

#### `function get( self, K key ) -> V`

Retrieve the value associated with the given key. 

#### `function contains( self, K key ) -> Boolean`

Check whether the given key exists in the mapping. 

#### `function length( self ) -> I64`

Return the number of entries in the mapping. 

#### `function isEmpty( self ) -> Boolean`

Check whether the mapping contains no entries. 

