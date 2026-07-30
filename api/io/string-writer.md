# uranite.io.string-writer

## Table of Contents

- [Imports](#imports)
- [class `StringWriter`](#class-stringwriter)
  - [`StringWriter()`](#StringWriter)
  - [`write()`](#write)
  - [`writeString()`](#writeString)
  - [`flush()`](#flush)
  - [`close()`](#close)
  - [`toString()`](#toString)
  - [`getLength()`](#getLength)
  - [`reset()`](#reset)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.stream`
  - `OutputStream`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.memory`
  - `Memory`

## class `StringWriter`

**Implements**: `OutputStream`

An in-memory output stream that accumulates written bytes into a dynamically growing buffer and can produce the result as a String. Implements the OutputStream interface. The internal buffer starts at 256 bytes and doubles in capacity when full. After calling toString, the buffer ownership transfers to the returned string and destroy must not be called.

### Fields

| Name | Type | Access |
|------|------|--------|
| `buffer` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `position` | `I64` | protect |
| `bufAddr` | `I64` | protect |

### Methods

#### `function StringWriter( self ) -> Void`

Construct a StringWriter with an initial buffer capacity of 256 bytes and the write position at zero.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function grow( self ) -> Void`

Double the buffer capacity and copy all existing content to the new buffer. The old buffer is freed after the copy.

**Complexity**:
- Time: `O(n) where n is the current position.`

#### `function write( self, Memory<I64> data, I64 offset, I64 length ) -> I64`

Write length bytes from the data buffer starting at offset into the internal buffer. Grows the buffer as needed to accommodate the data.

**Parameters**:

- `data` (`Memory<I64>`)
- `The source buffer containing bytes to write, with one`
- `byte value per slot.`
- `offset` (`I64`)
- `The starting index in the source buffer.`
- `length` (`I64`)
- `The number of bytes to write.`

**Returns**: — I64:
The number of bytes written, always equal to length.

**Complexity**:
- Time: `O(n) where n is the number of bytes written, amortized`

#### `function writeString( self, String text ) -> Void`

Write all bytes from a string into the internal buffer. Grows the buffer as needed.

**Parameters**:

- `text` (`String`)
- `The string whose bytes to append to the buffer.`

**Complexity**:
- Time: `O(n) where n is the length of the string, amortized`

#### `function flush( self ) -> Void`

Flush buffered output. This is a no-op for StringWriter since all writes go directly to the in-memory buffer, but is provided for OutputStream interface compatibility.

#### `function close( self ) -> Void`

Close the writer. This is a no-op for StringWriter since there are no external resources to release, but is provided for OutputStream interface compatibility.

#### `function toString( self ) -> String`

Convert the accumulated buffer contents to a String by null-terminating the buffer and reinterpreting the address as a string pointer. After calling this method, buffer ownership transfers to the returned string and destroy must not be called.

**Returns**: — String:
The accumulated bytes as a string.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getLength( self ) -> I64`

Return the number of bytes written so far.

**Returns**: `I64` — The current byte count in the buffer.

#### `function reset( self ) -> Void`

Reset the write position to zero, effectively discarding all previously written content without freeing the buffer. The buffer capacity is retained.

#### `function destroy( self ) -> Void`

Free the underlying memory buffer. Must not be called after toString has been invoked, since buffer ownership transfers to the returned string.

