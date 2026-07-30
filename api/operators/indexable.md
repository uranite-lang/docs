# uranite.operators.indexable

## Table of Contents

- [Imports](#imports)
- [interface `Indexable`](#interface-indexable)
  - [`get()`](#get)
  - [`set()`](#set)
  - [`remove()`](#remove)
  - [`contains()`](#contains)

## Imports

- `uranite.language.boolean`
  - `Boolean`

## interface `Indexable`<K, V>

Interface for types that support bracket-style element access (obj[key]). Implementing types must define how to retrieve and store values by key.

### Methods

#### `function get( self, K key ) -> V`

Retrieve the value associated with the given key. 

#### `function set( self, K key, V value ) -> Void`

Store the given value at the given key, replacing any existing value. 

#### `function remove( self, K key ) -> Void`

Delete the value associated with the given key. 

#### `function contains( self, K key ) -> Boolean`

Check whether the given key exists in this container. 

