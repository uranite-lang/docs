# uranite.regexp.match

## Table of Contents

- [Imports](#imports)
- [class `Match`](#class-match)
  - [`Match()`](#Match)
  - [`group()`](#group)
  - [`length()`](#length)
- [function `noMatch`](#function-nomatch)
  - [`noMatch()`](#noMatch)

## Imports

- `uranite.functions.builtin`
  - `substring`

## class `Match`

Result of a regex match operation. Contains the original text, the start and end positions of the matched region, and whether a match was found at all.

### Fields

| Name | Type | Access |
|------|------|--------|
| `text` | `String` | public |
| `start` | `I64` | public |
| `end` | `I64` | public |
| `matched` | `Boolean` | public |

### Methods

#### `function Match( self, String text, I64 start, I64 end, Boolean matched ) -> Void`

Construct a match result.

**Parameters**:

- `text` (`String`)
- `The original input text.`
- `start` (`I64`)
- `Byte offset where the match begins.`
- `end` (`I64`)
- `Byte offset where the match ends.`
- `matched` (`Boolean`)
- `Whether a match was found.`

#### `function group( self ) -> String`

Extract the matched substring from the original text.

**Returns**: — The substring from start to end of the match.

#### `function length( self ) -> I64`

Return the length of the matched region in bytes.

**Returns**: — The number of bytes in the match (end - start).

## function `noMatch`

Create a Match representing no match found.

**Returns**: — A Match with matched set to False, empty text, and zero positions.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function noMatch(  ) -> Match`

Create a Match representing no match found.

**Returns**: — A Match with matched set to False, empty text, and zero positions.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

