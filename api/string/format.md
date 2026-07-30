# uranite.string.format

## Table of Contents

- [Imports](#imports)
- [function `padRight`](#function-padright)
  - [`padRight()`](#padRight)
- [function `padLeft`](#function-padleft)
  - [`padLeft()`](#padLeft)
- [function `padZero`](#function-padzero)
  - [`padZero()`](#padZero)
- [function `formatUtcOffset`](#function-formatutcoffset)
  - [`formatUtcOffset()`](#formatUtcOffset)
- [function `appendTo`](#function-appendto)
  - [`appendTo()`](#appendTo)

## Imports

- `uranite.convert.convert`
  - `intToString`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`

## function `padRight`

Pad a string with trailing space characters (0x20) to reach the given width. Returns the original string unchanged if its length is already equal to or greater than width.

**Parameters**:

- `source` (`String`)
- `The input string to pad.`
- `width` (`I64`)
- `The desired minimum total width.`

**Returns**: — String:
A new string of exactly width characters with trailing spaces,
or the original string if already wide enough.

**Complexity**:
- Time: `O(width)`
- Space: `O(width) for the allocated buffer.`

### Methods

#### `function padRight( String source, I64 width ) -> String`

Pad a string with trailing space characters (0x20) to reach the given width. Returns the original string unchanged if its length is already equal to or greater than width.

**Parameters**:

- `source` (`String`)
- `The input string to pad.`
- `width` (`I64`)
- `The desired minimum total width.`

**Returns**: — String:
A new string of exactly width characters with trailing spaces,
or the original string if already wide enough.

**Complexity**:
- Time: `O(width)`
- Space: `O(width) for the allocated buffer.`

## function `padLeft`

Pad a string with leading space characters (0x20) to reach the given width. Returns the original string unchanged if its length is already equal to or greater than width.

**Parameters**:

- `source` (`String`)
- `The input string to pad.`
- `width` (`I64`)
- `The desired minimum total width.`

**Returns**: — String:
A new string of exactly width characters with leading spaces,
or the original string if already wide enough.

**Complexity**:
- Time: `O(width)`
- Space: `O(width) for the allocated buffer.`

### Methods

#### `function padLeft( String source, I64 width ) -> String`

Pad a string with leading space characters (0x20) to reach the given width. Returns the original string unchanged if its length is already equal to or greater than width.

**Parameters**:

- `source` (`String`)
- `The input string to pad.`
- `width` (`I64`)
- `The desired minimum total width.`

**Returns**: — String:
A new string of exactly width characters with leading spaces,
or the original string if already wide enough.

**Complexity**:
- Time: `O(width)`
- Space: `O(width) for the allocated buffer.`

## function `padZero`

Format a non-negative integer with leading ASCII zero characters (0x30) to reach the given width. If the string representation of value is already at or beyond the target width, it is returned as-is without truncation.

**Parameters**:

- `value` (`I64`)
- `The integer value to format` (`should be non-negative`)
- `width` (`I64`)
- `The minimum width of the resulting string.`

**Returns**: — String:
A zero-padded string, e.g. padZero(5, 3) returns "005".

**Complexity**:
- Time: `O(width)`
- Space: `O(width) for the allocated buffer.`

### Methods

#### `function padZero( I64 value, I64 width ) -> String`

Format a non-negative integer with leading ASCII zero characters (0x30) to reach the given width. If the string representation of value is already at or beyond the target width, it is returned as-is without truncation.

**Parameters**:

- `value` (`I64`)
- `The integer value to format` (`should be non-negative`)
- `width` (`I64`)
- `The minimum width of the resulting string.`

**Returns**: — String:
A zero-padded string, e.g. padZero(5, 3) returns "005".

**Complexity**:
- Time: `O(width)`
- Space: `O(width) for the allocated buffer.`

## function `formatUtcOffset`

Format a UTC offset given in seconds into the standard ISO 8601 string format +HH:MM or -HH:MM.

**Parameters**:

- `offsetSeconds` (`I64`)
- `The UTC offset in seconds. Positive values are east of UTC,`
- `negative values are west. For example, 25200 represents +07`
- `and -18000 represents -05`

**Returns**: — String:
The formatted offset string, e.g. "+07:00" or "-05:00".

**Complexity**:
- Time: `O(1)`
- Space: `O(1) for the short result string.`

### Methods

#### `function formatUtcOffset( I64 offsetSeconds ) -> String`

Format a UTC offset given in seconds into the standard ISO 8601 string format +HH:MM or -HH:MM.

**Parameters**:

- `offsetSeconds` (`I64`)
- `The UTC offset in seconds. Positive values are east of UTC,`
- `negative values are west. For example, 25200 represents +07`
- `and -18000 represents -05`

**Returns**: — String:
The formatted offset string, e.g. "+07:00" or "-05:00".

**Complexity**:
- Time: `O(1)`
- Space: `O(1) for the short result string.`

## function `appendTo`

Append all bytes of a string into a pre-allocated buffer at the given byte position. Does not null-terminate — the caller is responsible for writing the terminator after all appends.

**Parameters**:

- `buffer` (`I64`)
- `The destination buffer base address.`
- `position` (`I64`)
- `The byte offset in buffer where the string will be written.`
- `content` (`String`)
- `The string whose bytes are appended.`

**Returns**: — I64:
The new position (position + length of content), ready for
the next append.

**Complexity**:
- Time: `O(n) where n is the byte length of content.`

### Methods

#### `function appendTo( I64 buffer, I64 position, String content ) -> I64`

Append all bytes of a string into a pre-allocated buffer at the given byte position. Does not null-terminate — the caller is responsible for writing the terminator after all appends.

**Parameters**:

- `buffer` (`I64`)
- `The destination buffer base address.`
- `position` (`I64`)
- `The byte offset in buffer where the string will be written.`
- `content` (`String`)
- `The string whose bytes are appended.`

**Returns**: — I64:
The new position (position + length of content), ready for
the next append.

**Complexity**:
- Time: `O(n) where n is the byte length of content.`

