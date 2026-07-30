# uranite.collection.hash-map

## Table of Contents

- [Imports](#imports)
- [class `HashMap`](#class-hashmap)
  - [`HashMap()`](#HashMap)
  - [`HashMap()`](#HashMap)
  - [`HashMap()`](#HashMap)
  - [`HashMap()`](#HashMap)
  - [`put()`](#put)
  - [`putIfAbsent()`](#putIfAbsent)
  - [`putAll()`](#putAll)
  - [`pop()`](#pop)
  - [`removeEntry()`](#removeEntry)
  - [`replace()`](#replace)
  - [`replaceEntry()`](#replaceEntry)
  - [`merge()`](#merge)
  - [`computeIfAbsent()`](#computeIfAbsent)
  - [`set()`](#set)
  - [`contains()`](#contains)
  - [`remove()`](#remove)
  - [`get()`](#get)
  - [`getOrDefault()`](#getOrDefault)
  - [`containsKey()`](#containsKey)
  - [`containsValue()`](#containsValue)
  - [`keys()`](#keys)
  - [`values()`](#values)
  - [`entries()`](#entries)
  - [`forEach()`](#forEach)
  - [`append()`](#append)
  - [`clear()`](#clear)
  - [`copy()`](#copy)
  - [`empty()`](#empty)
  - [`equals()`](#equals)
  - [`exists()`](#exists)
  - [`filter()`](#filter)
  - [`length()`](#length)
  - [`remove()`](#remove)
  - [`iterator()`](#iterator)
  - [`toString()`](#toString)
  - [`size()`](#size)
  - [`isEmpty()`](#isEmpty)
  - [`isNotEmpty()`](#isNotEmpty)
  - [`mapValues()`](#mapValues)
  - [`filterEntries()`](#filterEntries)
  - [`any()`](#any)
  - [`all()`](#all)
  - [`destroy()`](#destroy)
- [class `HashMapIterator`](#class-hashmapiterator)
  - [`HashMapIterator()`](#HashMapIterator)
  - [`has()`](#has)
  - [`next()`](#next)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.collection.collection`
  - `Collection`
- `uranite.collection.hash-set`
  - `HashSet`
- `uranite.collection.map`
  - `Map`
- `uranite.collection.mutable-mapping`
  - `MutableMapping`
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
- `uranite.language.string`
  - `String`
- `uranite.language.void`
  - `Void`
- `uranite.memory.memory`
  - `Memory`

## class `HashMap`<K, V>

**Implements**: `MutableMapping<K, V>`

Hash-based associative map using open addressing with linear probing.

Keys are hashed to determine bucket placement. Collisions are resolved by linear probing, and deleted slots are marked as tombstones (status 2) to maintain probe chain integrity. The table automatically grows when the load factor exceeds 75%.

### Fields

| Name | Type | Access |
|------|------|--------|
| `keyData` | `Memory<K>` | private |
| `valueData` | `Memory<V>` | private |
| `status` | `Memory<Int>` | private |
| `count` | `Int` | private |
| `capacity` | `Int` | private |

### Methods

#### `function HashMap( self ) -> Void`

Create an empty map with a default initial capacity of 16 buckets.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function HashMap( self, Int initialCapacity ) -> Void`

Create an empty map with the specified initial capacity.

**Parameters**:

- `initialCapacity` (`Int`)
- `The number of buckets to pre-allocate.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function HashMap( self, Sequence<Pair<K, V>> pairs ) -> Void`

Create a map populated with all key-value pairs from the given sequence.

**Parameters**:

- `pairs` (`Sequence<Pair<K, V>>`)
- `The sequence of pairs to insert into this map.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function HashMap( self, Int initialCapacity, Sequence<Pair<K, V>> pairs ) -> Void`

Create a map with the specified capacity, populated from the given pairs.

**Parameters**:

- `initialCapacity` (`Int`)
- `The number of buckets to pre-allocate.`
- `pairs` (`Sequence<Pair<K, V>>`)
- `The sequence of pairs to insert into this map.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function computeIndex( self, K key ) -> Int`

Compute the bucket index from the key's hash code, wrapping negative values to ensure a positive index within the capacity range.

**Parameters**:

- `key` (`K`)
- `The key to compute the index for.`

**Returns**: — Int:
The non-negative bucket index.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function probeKey( self, K key ) -> Int`

Perform linear probing to find the slot for the given key, reusing the first deleted tombstone encountered during the probe.

**Parameters**:

- `key` (`K`)
- `The key to probe for.`

**Returns**: — Int:
The slot index where the key should be placed, or -1 if the
table is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findKey( self, K key ) -> Int`

Search for an existing key in the table using linear probing.

**Parameters**:

- `key` (`K`)
- `The key to search for.`

**Returns**: — Int:
The slot index if the key is found, or -1 if absent.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function grow( self ) -> Void`

Double the table capacity and rehash all existing entries into the new buckets.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function put( self, K key, V value ) -> Void`

Insert or update a key-value pair. If the key already exists, its value is replaced. The table grows automatically when the load factor exceeds 75%.

**Parameters**:

- `key` (`K`)
- `The key to insert or update.`
- `value` (`V`)
- `The value to associate with the key.`

**Complexity**:
- Time: `O(1) amortized, O(n) worst case`
- Space: `O(1) amortized`

#### `function putIfAbsent( self, K key, V value ) -> V`

Insert the key-value pair only if the key is not already present.

**Parameters**:

- `key` (`K`)
- `The key to insert.`
- `value` (`V`)
- `The value to associate if the key is absent.`

**Returns**: — V:
The existing value if the key was present, or the new value if inserted.

**Complexity**:
- Time: `O(1) amortized, O(n) worst case`
- Space: `O(1) amortized`

#### `function putAll( self, Map<K, V> source ) -> Void`

Copy all entries from the source map into this map.

**Parameters**:

- `source` (`Map<K, V>`)
- `The map whose entries are to be copied.`

**Complexity**:
- Time: `O(m) amortized where m is source size`
- Space: `O(1) amortized`

#### `function pop( self, K key ) -> V`

Remove the entry with the given key and return its value.

**Parameters**:

- `key` (`K`)
- `The key of the entry to remove.`

**Returns**: `V` — The value that was associated with the removed key.

**Raises**:

- `KeyError` → `LookupError` → `Error` — If the key is not present in the map.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function removeEntry( self, K key, V value ) -> Boolean`

Remove the entry only if the key currently maps to the given value.

**Parameters**:

- `key` (`K`)
- `The key to check.`
- `value` (`V`)
- `The expected value that must match for removal.`

**Returns**: — Boolean:
True if the entry was found with the matching value and removed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function replace( self, K key, V value ) -> V`

Replace the value for an existing key and return the old value.

**Parameters**:

- `key` (`K`)
- `The key whose value is to be replaced.`
- `value` (`V`)
- `The new value.`

**Returns**: `V` — The previous value associated with the key.

**Raises**:

- `KeyError` → `LookupError` → `Error` — If the key is not present in the map.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function replaceEntry( self, K key, V oldValue, V newValue ) -> Boolean`

Replace the value only if the key currently maps to the expected old value.

**Parameters**:

- `key` (`K`)
- `The key to check.`
- `oldValue` (`V`)
- `The expected current value.`
- `newValue` (`V`)
- `The new value to set if the current value matches.`

**Returns**: — Boolean:
True if the replacement was performed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function merge( self, Map<K, V> other ) -> Void`

Merge all entries from another map into this map, overwriting existing keys with the values from the other map.

**Parameters**:

- `other` (`Map<K, V>`)
- `The map to merge from.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function computeIfAbsent( self, K key, <type> mapping ) -> V`

Compute and insert a value using the mapping function if the key is absent.

**Parameters**:

- `key` (`K`)
- `The key to check and potentially insert for.`
- `mapping` (`Callable<V,<K>>`)
- `The function to compute the value from the key.`

**Returns**: — V:
The existing value if present, or the newly computed value.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function set( self, K key, V value ) -> Void`

Store the given value at the given key, replacing any existing value.

**Parameters**:

- `key` (`K`)
- `The key to associate with the value.`
- `value` (`V`)
- `The value to store.`

#### `function contains( self, K key ) -> Boolean`

Check whether the given key exists in the map.

**Parameters**:

- `key` (`K`)
- `The key to search for.`

**Returns**: — Boolean:
True if the key is present, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function remove( self, K key ) -> Void`

Remove the entry with the given key without returning its value.

**Parameters**:

- `key` (`K`)
- `The key of the entry to remove.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function get( self, K key ) -> V`

Retrieve the value associated with the given key.

**Parameters**:

- `key` (`K`)
- `The key to look up.`

**Returns**: `V` — The value associated with the key.

**Raises**:

- `KeyError` → `LookupError` → `Error` — If the key is not present in the map.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getOrDefault( self, K key, V defaultValue ) -> V`

Retrieve the value for the given key, or return the default value if the key is not present.

**Parameters**:

- `key` (`K`)
- `The key to look up.`
- `defaultValue` (`V`)
- `The value to return if the key is absent.`

**Returns**: — V:
The associated value, or the default.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function containsKey( self, K key ) -> Boolean`

Check whether the given key exists in the map.

**Parameters**:

- `key` (`K`)
- `The key to search for.`

**Returns**: — Boolean:
True if the key is present, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function containsValue( self, V value ) -> Boolean`

Check whether the given value exists in the map via linear scan of all occupied slots.

**Parameters**:

- `value` (`V`)
- `The value to search for.`

**Returns**: — Boolean:
True if the value is found, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property keys( self ) -> Set<K>`

`property` 

Return a set containing all keys currently in the map.

**Returns**: — Set<K>:
A HashSet of all keys.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property values( self ) -> Sequence<V>`

`property` 

Return a sequence containing all values currently in the map.

**Returns**: — Sequence<V>:
An ArrayList of all values.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property entries( self ) -> Set<Pair<K, V>>`

`property` 

Return a set of all key-value pairs currently in the map.

**Returns**: — Set<Pair<K, V>>:
A HashSet of Pair objects representing each entry.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function forEach( self, <type> action ) -> Void`

Execute an action for each key-value pair in the map.

**Parameters**:

- `action` (`Callable<Void,<K,V>>`)
- `The action to execute, receiving the key and value as arguments.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function append( self, V element ) -> Collection<V>`

No-op for map collections; returns self unchanged.

**Complexity**:
- Time: `O(1) amortized, O(n) worst case`
- Space: `O(1) amortized`

#### `function clear( self ) -> Void`

Remove all entries from the map by resetting all slot statuses to empty.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property copy( self ) -> Collection<V>`

`property` 

Return a shallow copy of this map with the same entries.

**Returns**: — Collection<V>:
A new HashMap containing copies of all key-value pairs.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property empty( self ) -> Boolean`

`property` 

Check whether the map contains no entries.

**Returns**: `Boolean` — True if the map has zero entries, False otherwise.

#### `function equals( self, Object object ) -> Boolean`

Compare this map with another object for structural equality of all entries.

**Parameters**:

- `object` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is a HashMap with the same entries.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function exists( self, V element ) -> Boolean`

Check whether the given value exists in the map.

**Parameters**:

- `element` (`V`)
- `The value to search for.`

**Returns**: — Boolean:
True if the value is found, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function filter( self, <type> callback ) -> Collection<V>`

Return a new map containing only entries whose values match the predicate.

**Parameters**:

- `callback` (`Callable<Boolean,<V>>`)
- `A predicate function that returns True for values to include.`

**Returns**: — Collection<V>:
A new HashMap with only the matching entries.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property length( self ) -> Int`

`property` 

Return the number of entries currently in the map. 

#### `function remove( self, V element ) -> Boolean`

Remove the first entry with a matching value.

**Parameters**:

- `element` (`V`)
- `The value to match for removal.`

**Returns**: — Boolean:
True if an entry was found and removed, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property iterator( self ) -> Iterator<Pair<K, V>>`

`property` 

Return an iterator over the key-value pairs in the map.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toString( self ) -> String`

Return a string representation of the map in the format {key: value, ...}.

**Returns**: — String:
The formatted string representation.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `property size( self ) -> Int`

`property` 

Return the number of entries in the map.

Alias for the length property to match common collection conventions.

**Returns**: — Int:
The entry count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property isEmpty( self ) -> Boolean`

`property` 

Test whether the map contains no entries.

**Returns**: — Boolean:
True if the map has zero entries.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property isNotEmpty( self ) -> Boolean`

`property` 

Test whether the map contains at least one entry.

**Returns**: — Boolean:
True if the map has one or more entries.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function mapValues( self, <type> transform ) -> HashMap<K, V>`

Return a new map with the same keys but values transformed by the given function.

**Parameters**:

- `transform` (`Callable<V, <V>>`)
- `Function applied to each value to produce the new value.`

**Returns**: — HashMap<K, V>:
New map with transformed values.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function filterEntries( self, <type> predicate ) -> HashMap<K, V>`

Return a new map containing only entries where the predicate returns True for the key-value pair.

**Parameters**:

- `predicate` (`Callable<Boolean, <K, V>>`)
- `Function receiving key and value, returning True to include.`

**Returns**: — HashMap<K, V>:
New map with only matching entries.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function any( self, <type> predicate ) -> Boolean`

Test whether at least one entry satisfies the predicate.

Short-circuits on the first match.

**Parameters**:

- `predicate` (`Callable<Boolean, <K, V>>`)
- `Function receiving key and value, returning True for a match.`

**Returns**: — Boolean:
True if any entry satisfies the predicate.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function all( self, <type> predicate ) -> Boolean`

Test whether every entry satisfies the predicate.

Short-circuits on the first non-matching entry.

**Parameters**:

- `predicate` (`Callable<Boolean, <K, V>>`)
- `Function receiving key and value, returning True for conformance.`

**Returns**: — Boolean:
True if all entries satisfy the predicate.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release all heap-allocated memory buffers held by this map.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `HashMapIterator`<K, V>

**Implements**: `Iterator<Pair<K, V>>`

Iterator that traverses occupied slots in a HashMap, yielding key-value pairs as Pair<K, V> instances.

### Fields

| Name | Type | Access |
|------|------|--------|
| `keyData` | `Memory<K>` | private |
| `valueData` | `Memory<V>` | private |
| `status` | `Memory<Int>` | private |
| `capacity` | `Int` | private |
| `position` | `Int` | private |

### Methods

#### `function HashMapIterator( self, Memory<K> keyData, Memory<V> valueData, Memory<Int> status, Int capacity ) -> Void`

Create an iterator over the given key and value arrays, starting at the first occupied slot.

**Parameters**:

- `keyData`
- `valueData`
- `status: The slot status array` (`0=empty, 1=occupied, 2=deleted`)
- `capacity`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property has( self ) -> Boolean`

`property` 

Check whether more entries remain to be iterated.

**Returns**: `Boolean` — True if the iterator has not yet reached the end of the table.

#### `property next( self ) -> Pair<K, V>`

`property` 

Return the current key-value pair and advance to the next occupied slot.

**Returns**: — Pair<K, V>:
The key-value pair at the current position before advancing.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

