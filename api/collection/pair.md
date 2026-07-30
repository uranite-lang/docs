# uranite.collection.pair

## Table of Contents

- [Imports](#imports)
- [struct `Pair`](#struct-pair)
  - [`Pair()`](#Pair)
  - [`equals()`](#equals)
  - [`toString()`](#toString)

## Imports

- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.string`
  - `String`

## struct `Pair`<K, V>

Immutable key-value pair associating a key of type K with a value of type V.

Pairs are commonly used as entries in maps and as return values when two related values need to be grouped together without defining a dedicated type.

### Fields

| Name | Type | Access |
|------|------|--------|
| `key` | `K` | public |
| `value` | `V` | public |

### Methods

#### `function Pair( self, K key, V value ) -> Void`

Create a new pair from the given key and value.

**Parameters**:

- `key` (`K`)
- `The key component of the pair.`
- `value` (`V`)
- `The value component of the pair.`

#### `function equals( self, Object other ) -> Boolean`

Compare this pair with another object for structural equality.

Two pairs are considered equal if both their key and value components are equal. If the other object is not a Pair, this method returns False.

**Parameters**:

- `other` (`Object`)
- `The object to compare against.`

**Returns**: `Boolean` — True if the other object is a Pair with equal key and value, False otherwise.

#### `function toString( self ) -> String`

Return a string representation of this pair in the format Pair(key, value).

**Returns**: `String` — A human-readable string showing both components of the pair.

