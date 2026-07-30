# uranite.functions.builtin

## Table of Contents

- [Imports](#imports)
- [function `length`](#function-length)
  - [`length()`](#length)
- [function `charAt`](#function-charat)
  - [`charAt()`](#charAt)
- [function `equals`](#function-equals)
  - [`equals()`](#equals)
- [function `contains`](#function-contains)
  - [`contains()`](#contains)
- [function `indexOf`](#function-indexof)
  - [`indexOf()`](#indexOf)
- [function `lastIndexOf`](#function-lastindexof)
  - [`lastIndexOf()`](#lastIndexOf)
- [function `startsWith`](#function-startswith)
  - [`startsWith()`](#startsWith)
- [function `endsWith`](#function-endswith)
  - [`endsWith()`](#endsWith)
- [function `substring`](#function-substring)
  - [`substring()`](#substring)
- [function `trim`](#function-trim)
  - [`trim()`](#trim)
- [function `toUpper`](#function-toupper)
  - [`toUpper()`](#toUpper)
- [function `toLower`](#function-tolower)
  - [`toLower()`](#toLower)
- [function `isDigit`](#function-isdigit)
  - [`isDigit()`](#isDigit)
- [function `isAlpha`](#function-isalpha)
  - [`isAlpha()`](#isAlpha)
- [function `isAlphaNumeric`](#function-isalphanumeric)
  - [`isAlphaNumeric()`](#isAlphaNumeric)
- [function `isWhitespace`](#function-iswhitespace)
  - [`isWhitespace()`](#isWhitespace)
- [function `repeat`](#function-repeat)
  - [`repeat()`](#repeat)
- [function `reverse`](#function-reverse)
  - [`reverse()`](#reverse)
- [function `concat`](#function-concat)
  - [`concat()`](#concat)
- [function `charToString`](#function-chartostring)
  - [`charToString()`](#charToString)
- [function `replaceFirst`](#function-replacefirst)
  - [`replaceFirst()`](#replaceFirst)
- [function `padStart`](#function-padstart)
  - [`padStart()`](#padStart)
- [function `padEnd`](#function-padend)
  - [`padEnd()`](#padEnd)
- [function `split`](#function-split)
  - [`split()`](#split)
- [function `join`](#function-join)
  - [`join()`](#join)
- [function `replace`](#function-replace)
  - [`replace()`](#replace)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.io.syscall`
  - `ptrToString`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`
- `uranite.memory.memory`
  - `Memory`
- `uranite.string.builder`
  - `StringBuilder`

## function `length`

Return the byte length of a string.

**Parameters**:

- `content` (`String`)
- `The string to measure.`

**Returns**: — The number of bytes in the string (excluding null terminator).

### Methods

#### `function length( String content ) -> I64`

Return the byte length of a string.

**Parameters**:

- `content` (`String`)
- `The string to measure.`

**Returns**: — The number of bytes in the string (excluding null terminator).

## function `charAt`

Return the byte value at the given index in a string.

**Parameters**:

- `content` (`String`)
- `The string to index into.`
- `index` (`I64`)
- `Zero-based byte offset.`

**Returns**: — The byte value at the given index.

### Methods

#### `function charAt( String content, I64 index ) -> I64`

Return the byte value at the given index in a string.

**Parameters**:

- `content` (`String`)
- `The string to index into.`
- `index` (`I64`)
- `Zero-based byte offset.`

**Returns**: — The byte value at the given index.

## function `equals`

Compare two strings for byte-level equality.

**Parameters**:

- `first` (`String`)
- `The first string.`
- `second` (`String`)
- `The second string.`

**Returns**: — True if both strings have the same length and identical bytes.

**Complexity**:
- Time: `O(n) where n is the string length`

### Methods

#### `function equals( String first, String second ) -> Boolean`

Compare two strings for byte-level equality.

**Parameters**:

- `first` (`String`)
- `The first string.`
- `second` (`String`)
- `The second string.`

**Returns**: — True if both strings have the same length and identical bytes.

**Complexity**:
- Time: `O(n) where n is the string length`

## function `contains`

Check whether the needle string appears anywhere within the haystack.

**Parameters**:

- `haystack` (`String`)
- `The string to search in.`
- `needle` (`String`)
- `The string to search for.`

**Returns**: — True if needle is found within haystack.

### Methods

#### `function contains( String haystack, String needle ) -> Boolean`

Check whether the needle string appears anywhere within the haystack.

**Parameters**:

- `haystack` (`String`)
- `The string to search in.`
- `needle` (`String`)
- `The string to search for.`

**Returns**: — True if needle is found within haystack.

## function `indexOf`

Find the first occurrence of needle within haystack.

**Parameters**:

- `haystack` (`String`)
- `The string to search in.`
- `needle` (`String`)
- `The string to search for.`

**Returns**: — The byte offset of the first occurrence, or -1 if not found.
Returns 0 for an empty needle.

**Complexity**:
- Time: `O(n * m) where n is haystack length and m is needle length`

### Methods

#### `function indexOf( String haystack, String needle ) -> I64`

Find the first occurrence of needle within haystack.

**Parameters**:

- `haystack` (`String`)
- `The string to search in.`
- `needle` (`String`)
- `The string to search for.`

**Returns**: — The byte offset of the first occurrence, or -1 if not found.
Returns 0 for an empty needle.

**Complexity**:
- Time: `O(n * m) where n is haystack length and m is needle length`

## function `lastIndexOf`

Find the last occurrence of needle within haystack.

**Parameters**:

- `haystack` (`String`)
- `The string to search in.`
- `needle` (`String`)
- `The string to search for.`

**Returns**: — The byte offset of the last occurrence, or -1 if not found.
Returns the haystack length for an empty needle.

**Complexity**:
- Time: `O(n * m) where n is haystack length and m is needle length`

### Methods

#### `function lastIndexOf( String haystack, String needle ) -> I64`

Find the last occurrence of needle within haystack.

**Parameters**:

- `haystack` (`String`)
- `The string to search in.`
- `needle` (`String`)
- `The string to search for.`

**Returns**: — The byte offset of the last occurrence, or -1 if not found.
Returns the haystack length for an empty needle.

**Complexity**:
- Time: `O(n * m) where n is haystack length and m is needle length`

## function `startsWith`

Check whether a string starts with the given prefix.

**Parameters**:

- `content` (`String`)
- `The string to check.`
- `prefix` (`String`)
- `The prefix to test for.`

**Returns**: — True if content begins with prefix.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function startsWith( String content, String prefix ) -> Boolean`

Check whether a string starts with the given prefix.

**Parameters**:

- `content` (`String`)
- `The string to check.`
- `prefix` (`String`)
- `The prefix to test for.`

**Returns**: — True if content begins with prefix.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `endsWith`

Check whether a string ends with the given suffix.

**Parameters**:

- `content` (`String`)
- `The string to check.`
- `suffix` (`String`)
- `The suffix to test for.`

**Returns**: — True if content ends with suffix.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function endsWith( String content, String suffix ) -> Boolean`

Check whether a string ends with the given suffix.

**Parameters**:

- `content` (`String`)
- `The string to check.`
- `suffix` (`String`)
- `The suffix to test for.`

**Returns**: — True if content ends with suffix.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `substring`

Extract a substring by byte range with clamping.

**Parameters**:

- `content` (`String`)
- `The source string.`
- `start` (`I64`)
- `Starting byte offset` (`clamped to 0 if negative`)
- `end` (`I64`)
- `Ending byte offset exclusive` (`clamped to string length`)

**Returns**: — The extracted substring, or empty string if start >= end.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function substring( String content, I64 start, I64 end ) -> String`

Extract a substring by byte range with clamping.

**Parameters**:

- `content` (`String`)
- `The source string.`
- `start` (`I64`)
- `Starting byte offset` (`clamped to 0 if negative`)
- `end` (`I64`)
- `Ending byte offset exclusive` (`clamped to string length`)

**Returns**: — The extracted substring, or empty string if start >= end.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `trim`

Remove leading and trailing ASCII whitespace (space, tab, newline, CR).

**Parameters**:

- `content` (`String`)
- `The string to trim.`

**Returns**: — The trimmed string, or empty string if content is all whitespace.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function trim( String content ) -> String`

Remove leading and trailing ASCII whitespace (space, tab, newline, CR).

**Parameters**:

- `content` (`String`)
- `The string to trim.`

**Returns**: — The trimmed string, or empty string if content is all whitespace.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `toUpper`

Convert all lowercase ASCII letters (a-z) to uppercase (A-Z).

**Parameters**:

- `content` (`String`)
- `The string to convert.`

**Returns**: — A new string with lowercase letters uppercased.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function toUpper( String content ) -> String`

Convert all lowercase ASCII letters (a-z) to uppercase (A-Z).

**Parameters**:

- `content` (`String`)
- `The string to convert.`

**Returns**: — A new string with lowercase letters uppercased.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `toLower`

Convert all uppercase ASCII letters (A-Z) to lowercase (a-z).

**Parameters**:

- `content` (`String`)
- `The string to convert.`

**Returns**: — A new string with uppercase letters lowercased.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function toLower( String content ) -> String`

Convert all uppercase ASCII letters (A-Z) to lowercase (a-z).

**Parameters**:

- `content` (`String`)
- `The string to convert.`

**Returns**: — A new string with uppercase letters lowercased.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `isDigit`

Return True if the byte value is an ASCII digit (0x30-0x39).

### Methods

#### `function isDigit( I64 codePoint ) -> Boolean`

Return True if the byte value is an ASCII digit (0x30-0x39).

## function `isAlpha`

Return True if the byte value is an ASCII letter (A-Z or a-z).

### Methods

#### `function isAlpha( I64 codePoint ) -> Boolean`

Return True if the byte value is an ASCII letter (A-Z or a-z).

## function `isAlphaNumeric`

Return True if the byte value is an ASCII letter or digit.

### Methods

#### `function isAlphaNumeric( I64 codePoint ) -> Boolean`

Return True if the byte value is an ASCII letter or digit.

## function `isWhitespace`

Return True if the byte value is ASCII whitespace (space, tab, newline, CR).

### Methods

#### `function isWhitespace( I64 codePoint ) -> Boolean`

Return True if the byte value is ASCII whitespace (space, tab, newline, CR).

## function `repeat`

Repeat a string the specified number of times.

**Parameters**:

- `content` (`String`)
- `The string to repeat.`
- `count` (`I64`)
- `Number of repetitions. Returns empty string if <= 0.`

**Returns**: — The repeated string.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function repeat( String content, I64 count ) -> String`

Repeat a string the specified number of times.

**Parameters**:

- `content` (`String`)
- `The string to repeat.`
- `count` (`I64`)
- `Number of repetitions. Returns empty string if <= 0.`

**Returns**: — The repeated string.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `reverse`

Reverse a string byte-by-byte.

**Parameters**:

- `content` (`String`)
- `The string to reverse.`

**Returns**: — A new string with bytes in reverse order.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function reverse( String content ) -> String`

Reverse a string byte-by-byte.

**Parameters**:

- `content` (`String`)
- `The string to reverse.`

**Returns**: — A new string with bytes in reverse order.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `concat`

Concatenate two strings. Equivalent to the + operator.

### Methods

#### `function concat( String first, String second ) -> String`

Concatenate two strings. Equivalent to the + operator.

## function `charToString`

Convert a single byte value to a one-character string.

**Parameters**:

- `codePoint` (`I64`)
- `The byte value` (`0-255`)

**Returns**: — A single-character string containing the byte.

### Methods

#### `function charToString( I64 codePoint ) -> String`

Convert a single byte value to a one-character string.

**Parameters**:

- `codePoint` (`I64`)
- `The byte value` (`0-255`)

**Returns**: — A single-character string containing the byte.

## function `replaceFirst`

Replace only the first occurrence of target with replacement.

**Parameters**:

- `source` (`String`)
- `The original string.`
- `target` (`String`)
- `The substring to search for.`
- `replacement` (`String`)
- `The string to substitute for the first match.`

**Returns**: — String:
The string with the first occurrence replaced, or the
original string if target is not found.

**Complexity**:
- Time: `O(n * m) where n = source length, m = target length`

### Methods

#### `function replaceFirst( String source, String target, String replacement ) -> String`

Replace only the first occurrence of target with replacement.

**Parameters**:

- `source` (`String`)
- `The original string.`
- `target` (`String`)
- `The substring to search for.`
- `replacement` (`String`)
- `The string to substitute for the first match.`

**Returns**: — String:
The string with the first occurrence replaced, or the
original string if target is not found.

**Complexity**:
- Time: `O(n * m) where n = source length, m = target length`

## function `padStart`

Pad the source string from the start to reach the target length.

If the source is already at or beyond the target length, it is returned unchanged. The pad string is repeated as needed.

**Parameters**:

- `source` (`String`)
- `The string to pad.`
- `targetLength` (`I64`)
- `The desired minimum length.`
- `padString` (`String`)
- `The string to use for padding` (`typically a single character`)

**Returns**: — String:
The padded string.

**Complexity**:
- Time: `O(targetLength)`
- Space: `O(targetLength)`

### Methods

#### `function padStart( String source, I64 targetLength, String padString ) -> String`

Pad the source string from the start to reach the target length.

If the source is already at or beyond the target length, it is returned unchanged. The pad string is repeated as needed.

**Parameters**:

- `source` (`String`)
- `The string to pad.`
- `targetLength` (`I64`)
- `The desired minimum length.`
- `padString` (`String`)
- `The string to use for padding` (`typically a single character`)

**Returns**: — String:
The padded string.

**Complexity**:
- Time: `O(targetLength)`
- Space: `O(targetLength)`

## function `padEnd`

Pad the source string from the end to reach the target length.

If the source is already at or beyond the target length, it is returned unchanged. The pad string is repeated as needed.

**Parameters**:

- `source` (`String`)
- `The string to pad.`
- `targetLength` (`I64`)
- `The desired minimum length.`
- `padString` (`String`)
- `The string to use for padding` (`typically a single character`)

**Returns**: — String:
The padded string.

**Complexity**:
- Time: `O(targetLength)`
- Space: `O(targetLength)`

### Methods

#### `function padEnd( String source, I64 targetLength, String padString ) -> String`

Pad the source string from the end to reach the target length.

If the source is already at or beyond the target length, it is returned unchanged. The pad string is repeated as needed.

**Parameters**:

- `source` (`String`)
- `The string to pad.`
- `targetLength` (`I64`)
- `The desired minimum length.`
- `padString` (`String`)
- `The string to use for padding` (`typically a single character`)

**Returns**: — String:
The padded string.

**Complexity**:
- Time: `O(targetLength)`
- Space: `O(targetLength)`

## function `split`

Split a string by the given delimiter into a list of substrings.

If the delimiter is not found, returns a single-element list containing the original string. Empty segments between adjacent delimiters are preserved.

**Parameters**:

- `source` (`String`)
- `The string to split.`
- `delimiter` (`String`)
- `The separator to split on.`

**Returns**: — ArrayList<String>:
The list of substrings.

**Complexity**:
- Time: `O(n * m) where n is source length, m is delimiter length`
- Space: `O(n)`

### Methods

#### `function split( String source, String delimiter ) -> ArrayList<String>`

Split a string by the given delimiter into a list of substrings.

If the delimiter is not found, returns a single-element list containing the original string. Empty segments between adjacent delimiters are preserved.

**Parameters**:

- `source` (`String`)
- `The string to split.`
- `delimiter` (`String`)
- `The separator to split on.`

**Returns**: — ArrayList<String>:
The list of substrings.

**Complexity**:
- Time: `O(n * m) where n is source length, m is delimiter length`
- Space: `O(n)`

## function `join`

Join a list of strings with the given delimiter between each element.

**Parameters**:

- `delimiter` (`String`)
- `The separator to insert between elements.`
- `parts` (`ArrayList<String>`)
- `The list of strings to join.`

**Returns**: — String:
The concatenated result.

**Complexity**:
- Time: `O(n) where n is total character count across all parts`
- Space: `O(n)`

### Methods

#### `function join( String delimiter, ArrayList<String> parts ) -> String`

Join a list of strings with the given delimiter between each element.

**Parameters**:

- `delimiter` (`String`)
- `The separator to insert between elements.`
- `parts` (`ArrayList<String>`)
- `The list of strings to join.`

**Returns**: — String:
The concatenated result.

**Complexity**:
- Time: `O(n) where n is total character count across all parts`
- Space: `O(n)`

## function `replace`

Replace all occurrences of target in source with replacement.

**Parameters**:

- `source` (`String`)
- `The original string.`
- `target` (`String`)
- `The substring to find and replace.`
- `replacement` (`String`)
- `The string to substitute for each occurrence.`

**Returns**: — String:
The string with all replacements applied.

**Complexity**:
- Time: `O(n * m) where n is source length, m is target length`
- Space: `O(n)`

### Methods

#### `function replace( String source, String target, String replacement ) -> String`

Replace all occurrences of target in source with replacement.

**Parameters**:

- `source` (`String`)
- `The original string.`
- `target` (`String`)
- `The substring to find and replace.`
- `replacement` (`String`)
- `The string to substitute for each occurrence.`

**Returns**: — String:
The string with all replacements applied.

**Complexity**:
- Time: `O(n * m) where n is source length, m is target length`
- Space: `O(n)`

