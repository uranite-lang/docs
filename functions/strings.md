# uranite.functions.strings

## Table of Contents

- [Imports](#imports)
- [function `split`](#function-split)
  - [`split()`](#split)
- [function `joinStrings`](#function-joinstrings)
  - [`joinStrings()`](#joinStrings)
- [function `replace`](#function-replace)
  - [`replace()`](#replace)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.functions.builtin`
  - `indexOf`
  - `length`
  - `substring`

## function `split`

Split a string into an ArrayList of substrings separated by the delimiter.

An empty delimiter splits into individual characters. If the delimiter is not found, a single-element list containing the entire source is returned.

**Parameters**:

- `source` (`String`)
- `The string to split.`
- `delimiter` (`String`)
- `The separator string.`

**Returns**: — ArrayList<String>:
The list of substrings.

**Complexity**:
- Time: `O(n * m) where n = source length, m = delimiter length`

### Methods

#### `function split( String source, String delimiter ) -> ArrayList<String>`

Split a string into an ArrayList of substrings separated by the delimiter.

An empty delimiter splits into individual characters. If the delimiter is not found, a single-element list containing the entire source is returned.

**Parameters**:

- `source` (`String`)
- `The string to split.`
- `delimiter` (`String`)
- `The separator string.`

**Returns**: — ArrayList<String>:
The list of substrings.

**Complexity**:
- Time: `O(n * m) where n = source length, m = delimiter length`

## function `joinStrings`

Join all elements in an ArrayList into a single string with the given separator.

**Parameters**:

- `elements` (`ArrayList<String>`)
- `The list of strings to join.`
- `separator` (`String`)
- `The string placed between each pair of elements.`

**Returns**: — String:
The joined result, or empty string if the list is empty.

**Complexity**:
- Time: `O(n * k) where n = element count, k = average element length`

### Methods

#### `function joinStrings( ArrayList<String> elements, String separator ) -> String`

Join all elements in an ArrayList into a single string with the given separator.

**Parameters**:

- `elements` (`ArrayList<String>`)
- `The list of strings to join.`
- `separator` (`String`)
- `The string placed between each pair of elements.`

**Returns**: — String:
The joined result, or empty string if the list is empty.

**Complexity**:
- Time: `O(n * k) where n = element count, k = average element length`

## function `replace`

Replace all occurrences of target with replacement in the source string.

If target is not found, the original string is returned unchanged. If target is empty, the original string is returned unchanged.

**Parameters**:

- `source` (`String`)
- `The original string.`
- `target` (`String`)
- `The substring to search for.`
- `replacement` (`String`)
- `The string to substitute for each match.`

**Returns**: — String:
The string with all occurrences replaced.

**Complexity**:
- Time: `O(n * m) where n = source length, m = target length`

### Methods

#### `function replace( String source, String target, String replacement ) -> String`

Replace all occurrences of target with replacement in the source string.

If target is not found, the original string is returned unchanged. If target is empty, the original string is returned unchanged.

**Parameters**:

- `source` (`String`)
- `The original string.`
- `target` (`String`)
- `The substring to search for.`
- `replacement` (`String`)
- `The string to substitute for each match.`

**Returns**: — String:
The string with all occurrences replaced.

**Complexity**:
- Time: `O(n * m) where n = source length, m = target length`

