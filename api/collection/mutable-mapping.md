# uranite.collection.mutable-mapping

## Table of Contents

- [Imports](#imports)
- [interface `MutableMapping`](#interface-mutablemapping)
  - [`put()`](#put)
  - [`putIfAbsent()`](#putIfAbsent)
  - [`putAll()`](#putAll)
  - [`pop()`](#pop)
  - [`removeEntry()`](#removeEntry)
  - [`replace()`](#replace)
  - [`replaceEntry()`](#replaceEntry)
  - [`clear()`](#clear)
  - [`merge()`](#merge)
  - [`computeIfAbsent()`](#computeIfAbsent)

## Imports

- `uranite.collection.map`
  - `Map`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.void`
  - `Void`

## interface `MutableMapping`<K, V>

**Implements**: `Map<K, V>`

Map that supports in-place insertion, removal, and update of entries.

This interface extends Map with mutating operations such as put, remove, replace, and merge, allowing concrete map implementations to modify their contents after construction.

### Methods

#### `function put( self, K key, V value ) -> Void`

Insert or update the value associated with the given key.

If the key already exists in the map, its value is replaced with the new value. If the key does not exist, a new entry is created.

**Parameters**:

- `key` (`K`)
- `The key to associate with the given value.`
- `value` (`V`)
- `The value to store for the given key.`

#### `function putIfAbsent( self, K key, V value ) -> V`

Insert a key-value pair only if the key is not already present in the map.

If the key already exists, the existing value is returned without modification. If the key is absent, the new value is inserted and returned.

**Parameters**:

- `key` (`K`)
- `The key to check and potentially insert.`
- `value` (`V`)
- `The value to insert if the key is absent.`

**Returns**: `V` — The existing value if the key was present, or the newly inserted value.

#### `function putAll( self, Map<K, V> entries ) -> Void`

Insert all entries from another map into this map.

For each entry in the provided map, if the key already exists in this map, its value is replaced. If the key does not exist, a new entry is created.

**Parameters**:

- `entries` (`Map<K, V>`)
- `The map whose entries will be inserted into this map.`

#### `function pop( self, K key ) -> V`

Remove the entry associated with the given key and return its value.

**Parameters**:

- `key` (`K`)
- `The key whose entry should be removed.`

**Returns**: `V` — The value that was associated with the removed key.

#### `function removeEntry( self, K key, V value ) -> Boolean`

Remove the entry for the given key only if it currently maps to the specified value.

**Parameters**:

- `key` (`K`)
- `The key whose entry should be conditionally removed.`
- `value` (`V`)
- `The expected value that must match for removal to occur.`

**Returns**: `Boolean` — True if the entry was found with the matching value and removed, False otherwise.

#### `function replace( self, K key, V value ) -> V`

Replace the value for an existing key and return the old value.

**Parameters**:

- `key` (`K`)
- `The key whose value should be replaced.`
- `value` (`V`)
- `The new value to associate with the key.`

**Returns**: `V` — The previous value that was associated with the key.

#### `function replaceEntry( self, K key, V oldValue, V newValue ) -> Boolean`

Replace the value for a key only if it currently maps to the specified old value.

**Parameters**:

- `key` (`K`)
- `The key whose value should be conditionally replaced.`
- `oldValue` (`V`)
- `The expected current value that must match for replacement to occur.`
- `newValue` (`V`)
- `The new value to associate with the key if the old value matches.`

**Returns**: `Boolean` — True if the old value matched and was replaced, False otherwise.

#### `function clear( self ) -> Void`

Remove all entries from this map, leaving it empty. 

#### `function merge( self, Map<K, V> other ) -> Void`

Merge all entries from another map into this map.

For each entry in the other map, if the key already exists in this map, its value is replaced. If the key does not exist, a new entry is created.

**Parameters**:

- `other` (`Map<K, V>`)
- `The map whose entries will be merged into this map.`

#### `function computeIfAbsent( self, K key, <type> mapping ) -> V`

Compute and insert a value if the key is absent, using a mapping function.

If the key is already present, its existing value is returned without invoking the mapping function. If absent, the mapping function is called with the key to produce a value, which is then inserted and returned.

**Parameters**:

- `key` (`K`)
- `The key to check and potentially compute a value for.`
- `mapping` (`Callable<V,<K>>`)
- `A function that takes the key and produces a value to insert.`

**Returns**: `V` — The existing value if the key was present, or the newly computed value.

