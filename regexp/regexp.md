# uranite.regexp.regexp

## Table of Contents

- [Imports](#imports)
- [function `isWordChar`](#function-iswordchar)
- [function `isDigitChar`](#function-isdigitchar)
- [function `isSpaceChar`](#function-isspacechar)
- [function `matchCharClass`](#function-matchcharclass)
- [function `matchAtom`](#function-matchatom)
- [function `handleAlternation`](#function-handlealternation)
- [function `hasAlternation`](#function-hasalternation)
- [class `RegExp`](#class-regexp)
  - [`RegExp()`](#RegExp)
  - [`test()`](#test)
  - [`find()`](#find)
  - [`matches()`](#matches)
  - [`replaceFirst()`](#replaceFirst)
  - [`replaceAll()`](#replaceAll)
  - [`findFrom()`](#findFrom)
  - [`findAll()`](#findAll)
  - [`split()`](#split)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.functions.builtin`
  - `substring`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
- `uranite.regexp.errors`
  - `RegexSyntaxError`
- `uranite.regexp.match`
  - `Match`
  - `noMatch`

## function `isWordChar`

Return True if the byte is a word character (alphanumeric or underscore).

### Methods

#### `function isWordChar( I64 codePoint ) -> Boolean`

Return True if the byte is a word character (alphanumeric or underscore).

## function `isDigitChar`

Return True if the byte is an ASCII digit (0-9).

### Methods

#### `function isDigitChar( I64 codePoint ) -> Boolean`

Return True if the byte is an ASCII digit (0-9).

## function `isSpaceChar`

Return True if the byte is ASCII whitespace (space, tab, newline, CR, FF, VT).

### Methods

#### `function isSpaceChar( I64 codePoint ) -> Boolean`

Return True if the byte is ASCII whitespace (space, tab, newline, CR, FF, VT).

## function `matchCharClass`

Match a character class [...] against a single subject byte. Handles ranges (a-z), negation ([^...]), and literal characters.

**Parameters**:

- `patternPointer` (`I64`)
- `Raw pointer to the pattern bytes.`
- `patternLength` (`I64`)
- `Total length of the pattern.`
- `patternPosition` (`I64`)
- `Offset of the first byte after the opening bracket.`
- `subjectCodePoint` (`I64`)
- `The byte value to test against the class.`

**Returns**: — The pattern offset after the closing bracket if matched,
or -1 if the character does not match the class.

### Methods

#### `function matchCharClass( I64 patternPointer, I64 patternLength, I64 patternPosition, I64 subjectCodePoint ) -> I64`

Match a character class [...] against a single subject byte. Handles ranges (a-z), negation ([^...]), and literal characters.

**Parameters**:

- `patternPointer` (`I64`)
- `Raw pointer to the pattern bytes.`
- `patternLength` (`I64`)
- `Total length of the pattern.`
- `patternPosition` (`I64`)
- `Offset of the first byte after the opening bracket.`
- `subjectCodePoint` (`I64`)
- `The byte value to test against the class.`

**Returns**: — The pattern offset after the closing bracket if matched,
or -1 if the character does not match the class.

## function `matchAtom`

Recursive backtracking matcher. Attempts to match the pattern starting at patternOffset against the subject starting at subjectOffset. Handles literal bytes, dot wildcard, escape sequences, character classes, quantifiers (*, +, ?), and end-of-line anchor ($).

**Returns**: — The subject offset after the match on success, or -1 on failure.

### Methods

#### `function matchAtom( I64 patternPointer, I64 patternLength, I64 patternOffset, I64 subjectPointer, I64 subjectLength, I64 subjectOffset ) -> I64`

Recursive backtracking matcher. Attempts to match the pattern starting at patternOffset against the subject starting at subjectOffset. Handles literal bytes, dot wildcard, escape sequences, character classes, quantifiers (*, +, ?), and end-of-line anchor ($).

**Returns**: — The subject offset after the match on success, or -1 on failure.

## function `handleAlternation`

Handle pipe-delimited alternation in a pattern. Splits the pattern at top-level pipe characters and tries each branch from left to right, returning the first successful match.

**Returns**: — The subject offset after the match on success, or -1 if no branch matches.

### Methods

#### `function handleAlternation( I64 patternPointer, I64 patternLength, I64 subjectPointer, I64 subjectLength, I64 subjectOffset ) -> I64`

Handle pipe-delimited alternation in a pattern. Splits the pattern at top-level pipe characters and tries each branch from left to right, returning the first successful match.

**Returns**: — The subject offset after the match on success, or -1 if no branch matches.

## function `hasAlternation`

Check whether the pattern contains a top-level pipe (alternation) outside of any group.

### Methods

#### `function hasAlternation( I64 patternPointer, I64 patternLength ) -> Boolean`

Check whether the pattern contains a top-level pipe (alternation) outside of any group.

## class `RegExp`

Backtracking regular expression engine. Supports: dot (any char), quantifiers (*, +, ?), character classes ([...], [^...], ranges), anchors (^, $), escape sequences (\\d, \\w, \\s and negations), alternation (|), and literal matching. Compiles the pattern on construction and provides test, find, matches, replaceFirst, and replaceAll operations.

**Complexity**:
- Time: `O(2^n) worst case due to backtracking on pathological patterns, linear for simple patterns`

### Fields

| Name | Type | Access |
|------|------|--------|
| `pattern` | `String` | public |
| `patternPtr` | `I64` | public |
| `patternLen` | `I64` | public |
| `anchored` | `Boolean` | public |

### Methods

#### `function RegExp( self, String pattern ) -> Void`

Construct a regex from the given pattern string. Detects whether the pattern is anchored (starts with ^).

**Parameters**:

- `pattern` (`String`)
- `The regular expression pattern.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function test( self, String input ) -> Boolean`

Test whether the pattern matches anywhere in the input.

**Parameters**:

- `input` (`String`)
- `The subject string to search.`

**Returns**: — True if a match is found, False otherwise.

#### `function find( self, String input ) -> Match`

Find the first match of the pattern in the input string.

**Parameters**:

- `input` (`String`)
- `The subject string to search.`

**Returns**: — A Match containing the position and matched status.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function matches( self, String input ) -> Boolean`

Check whether the pattern matches the entire input string.

**Parameters**:

- `input` (`String`)
- `The subject string to test.`

**Returns**: — True if the entire string matches the pattern.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function replaceFirst( self, String input, String replacement ) -> String`

Replace the first occurrence of the pattern with the replacement.

**Parameters**:

- `input` (`String`)
- `The subject string.`
- `replacement` (`String`)
- `The string to substitute for the first match.`

**Returns**: — The input with the first match replaced, or the original
string if no match is found.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function replaceAll( self, String input, String replacement ) -> String`

Replace all non-overlapping occurrences of the pattern with the replacement. Includes a safety limit of 10000 iterations to prevent infinite loops on zero-length matches.

**Parameters**:

- `input` (`String`)
- `The subject string.`
- `replacement` (`String`)
- `The string to substitute for each match.`

**Returns**: — The input with all matches replaced.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findFrom( self, String input, I64 fromOffset ) -> Match`

Find the first match of the pattern starting at or after the given offset.

**Parameters**:

- `input` (`String`)
- `The subject string to search.`
- `fromOffset` (`I64`)
- `The byte offset to start searching from.`

**Returns**: — Match:
A Match containing the position and matched status, or a
no-match if no occurrence is found at or after fromOffset.

**Complexity**:
- Time: `O(n * m) where n is the remaining input length and m is the pattern length`

#### `function findAll( self, String input ) -> ArrayList<Match>`

Find all non-overlapping matches of the pattern in the input string.

Searches left to right. After each match, the next search begins at the end of the previous match. Zero-length matches advance by one byte to prevent infinite loops.

**Parameters**:

- `input` (`String`)
- `The subject string to search.`

**Returns**: — ArrayList<Match>:
A list of all non-overlapping Match objects found.

**Complexity**:
- Time: `O(n * m) where n is the input length and m is the pattern length`

#### `function split( self, String input ) -> ArrayList<String>`

Split the input string by all non-overlapping occurrences of the pattern.

The portions of the input between matches are returned as list elements. If the pattern is not found, a single-element list with the full input is returned.

**Parameters**:

- `input` (`String`)
- `The string to split.`

**Returns**: — ArrayList<String>:
The list of segments between matches.

**Complexity**:
- Time: `O(n * m) where n is the input length and m is the pattern length`

