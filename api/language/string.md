# uranite.language.string

## Table of Contents

- [Imports](#imports)
- [class `String`](#class-string)
  - [`String()`](#String)
  - [`getValue()`](#getValue)
  - [`toString()`](#toString)
  - [`length()`](#length)
  - [`isEmpty()`](#isEmpty)
  - [`concat()`](#concat)
  - [`equals()`](#equals)
  - [`startsWith()`](#startsWith)
  - [`endsWith()`](#endsWith)
  - [`contains()`](#contains)
  - [`toUpper()`](#toUpper)
  - [`toLower()`](#toLower)
  - [`trim()`](#trim)
  - [`replace()`](#replace)
  - [`split()`](#split)
  - [`hash()`](#hash)
  - [`format()`](#format)

## Imports

- `uranite.memory.allocator`
  - `alloc`
- `uranite.io.syscall`
  - `ptrToString`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`

## class `String`

Immutable sequence of characters. Provides methods for querying, transforming, and comparing string values.

### Fields

| Name | Type | Access |
|------|------|--------|
| `value` | `String` | protect |

### Methods

#### `function String( self, String value ) -> Void`

Construct a new String wrapping the given string value. 

#### `function getValue( self ) -> String`

Return the underlying string value. 

#### `function toString( self ) -> String`

Return this string value unchanged. 

#### `function length( self ) -> I64`

Return the number of characters in this string. 

#### `function isEmpty( self ) -> Boolean`

Return True if this string contains no characters. 

#### `function concat( self, String other ) -> String`

Return a new String formed by appending other to the end of self.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function equals( self, String other ) -> Boolean`

Return True if self and other contain the same sequence of characters. 

#### `function startsWith( self, String prefix ) -> Boolean`

Return True if this string begins with the given prefix. 

#### `function endsWith( self, String suffix ) -> Boolean`

Return True if this string ends with the given suffix. 

#### `function contains( self, String substr ) -> Boolean`

Return True if this string contains the given substring. 

#### `function toUpper( self ) -> String`

Return a new String with all characters converted to uppercase.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toLower( self ) -> String`

Return a new String with all characters converted to lowercase.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function trim( self ) -> String`

Return a new String with leading and trailing whitespace removed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function replace( self, String old, String replacement ) -> String`

Return a new String with all occurrences of old replaced by replacement.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function split( self, String delimiter ) -> String`

Split this string by the given delimiter and return the result. 

#### `function hash( self ) -> I64`

Return a hash code for this string. 

#### `function format( self, [I64] args[] ) -> String`

Format this string, replacing {} placeholders with positional arguments. 

