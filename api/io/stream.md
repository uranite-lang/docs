# uranite.io.stream

## Table of Contents

- [Imports](#imports)
- [interface `InputStream`](#interface-inputstream)
  - [`read()`](#read)
  - [`close()`](#close)
- [interface `OutputStream`](#interface-outputstream)
  - [`write()`](#write)
  - [`flush()`](#flush)
  - [`close()`](#close)
- [class `Stream`](#class-stream)
  - [`Stream()`](#Stream)

## Imports

- `uranite.memory.memory`
  - `Memory`

## interface `InputStream`

Interface for byte-oriented input streams. Implementations provide sequential read access to a source of bytes such as a file, socket, or in-memory buffer. Buffers use Memory<I64> with one byte value per slot.

### Methods

#### `function read( self, Memory<I64> buffer, I64 offset, I64 length ) -> I64`

Read up to length bytes from the stream into the buffer starting at the given offset. Returns the number of bytes actually read, which may be less than length if the end of the stream is reached. Returns 0 at end of stream.

**Parameters**:

- `buffer` (`Memory<I64>`)
- `The destination buffer to read bytes into, with one`
- `byte value per slot.`
- `offset` (`I64`)
- `The starting index in the buffer to write to.`
- `length` (`I64`)
- `The maximum number of bytes to read.`

**Returns**: `I64` — The number of bytes actually read, or 0 at end of stream.

#### `function close( self ) -> Void`

Close the stream and release any underlying resources such as file descriptors or memory buffers.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## interface `OutputStream`

Interface for byte-oriented output streams. Implementations provide sequential write access to a destination such as a file, socket, or in-memory buffer. Buffers use Memory<I64> with one byte value per slot.

### Methods

#### `function write( self, Memory<I64> buffer, I64 offset, I64 length ) -> I64`

Write up to length bytes from the buffer starting at the given offset to the stream. Returns the number of bytes actually written.

**Parameters**:

- `buffer` (`Memory<I64>`)
- `The source buffer containing bytes to write, with one`
- `byte value per slot.`
- `offset` (`I64`)
- `The starting index in the buffer to read from.`
- `length` (`I64`)
- `The number of bytes to write.`

**Returns**: `I64` — The number of bytes actually written.

#### `function flush( self ) -> Void`

Flush any buffered output to the underlying destination, ensuring all previously written bytes are committed.

#### `function close( self ) -> Void`

Close the stream and release any underlying resources. Any buffered data is flushed before closing.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Stream`

File-descriptor-backed output stream for directing text output to any writable file descriptor (stdout, stderr, files, pipes).

### Fields

| Name | Type | Access |
|------|------|--------|
| `fileDescriptor` | `I64` | public |

### Methods

#### `function Stream( self, I64 fileDescriptor ) -> Void`

Construct a stream targeting the given file descriptor.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The file descriptor to write to.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

