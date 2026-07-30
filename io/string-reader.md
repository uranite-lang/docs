# uranite.io.string-reader

## Table of Contents

- [Imports](#imports)
- [class `StringReader`](#class-stringreader)
  - [`StringReader()`](#StringReader)
  - [`read()`](#read)
  - [`close()`](#close)
  - [`getPosition()`](#getPosition)
  - [`getLength()`](#getLength)
  - [`readString()`](#readString)
  - [`reset()`](#reset)

## Imports

- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.stream`
  - `InputStream`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
- `uranite.memory.memory`
  - `Memory`

## class `StringReader`

**Implements**: `InputStream`

An in-memory input stream backed by a String. Provides sequential byte-level read access to the string content without any syscalls. Maintains a read position that advances with each read operation and can be reset to re-read from the beginning.

### Fields

| Name | Type | Access |
|------|------|--------|
| `data` | `String` | protect |
| `position` | `I64` | protect |
| `dataLength` | `I64` | protect |
| `dataPtr` | `I64` | protect |

### Methods

#### `function StringReader( self, String data ) -> Void`

Construct a StringReader that reads bytes from the given string. The read position starts at the beginning of the string.

**Parameters**:

- `data` (`String`)
- `The string to use as the data source.`

#### `function read( self, Memory<I64> buffer, I64 offset, I64 length ) -> I64`

Read up to length bytes from the string into the buffer starting at the given offset. The number of bytes read may be less than requested if the end of the string is reached. Returns 0 when no bytes remain.

**Parameters**:

- `buffer` (`Memory<I64>`)
- `The destination buffer to read bytes into, with one`
- `byte value per slot.`
- `offset` (`I64`)
- `The starting index in the buffer to write to.`
- `length` (`I64`)
- `The maximum number of bytes to read.`

**Returns**: — I64:
The number of bytes actually read.

**Complexity**:
- Time: `O(n) where n is the number of bytes read.`

#### `function close( self ) -> Void`

Close the reader. This is a no-op since StringReader holds no external resources, but is provided for interface compatibility with InputStream.

#### `function getPosition( self ) -> I64`

Return the current read position within the string.

**Returns**: `I64` — The byte offset of the next read operation.

#### `function getLength( self ) -> I64`

Return the total length of the source string in bytes.

**Returns**: `I64` — The string length.

#### `function readString( self, I64 maxLength ) -> String`

Read up to maxLength bytes from the current position and return as a String. Advances the read position.

**Parameters**:

- `maxLength` (`I64`)
- `Maximum number of bytes to read.`

**Returns**: — String:
The bytes read as a string. Empty string if at end.

**Complexity**:
- Time: `O(n) where n is bytes read`
- Space: `O(n)`

#### `function reset( self ) -> Void`

Reset the read position to the beginning of the string, allowing the content to be re-read from the start.

