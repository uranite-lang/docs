# uranite.io.buffered-reader

## Table of Contents

- [Imports](#imports)
- [class `BufferedReader`](#class-bufferedreader)
  - [`BufferedReader()`](#BufferedReader)
  - [`BufferedReader()`](#BufferedReader)
  - [`read()`](#read)
  - [`readInto()`](#readInto)
  - [`readLine()`](#readLine)
  - [`isEof()`](#isEof)
  - [`close()`](#close)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.file`
  - `File`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readByteAt`
  - `sysClose`
  - `sysRead`
  - `writeByteAt`
- `uranite.memory.memory`
  - `Memory`

## class `BufferedReader`

Buffered reader that wraps a raw file descriptor and reads data in configurable-size chunks for efficiency. Provides single-byte reads and line-oriented reading with automatic CR/LF handling. Uses raw sysRead syscalls to avoid interface dispatch overhead.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fd` | `I64` | protect |
| `rawBuffer` | `Memory<I64>` | protect |
| `rawAddr` | `I64` | protect |
| `bufferCapacity` | `I64` | protect |
| `bytesInBuffer` | `I64` | protect |
| `readPosition` | `I64` | protect |
| `closed` | `Boolean` | protect |
| `eof` | `Boolean` | protect |
| `ownsFd` | `Boolean` | protect |

### Methods

#### `function BufferedReader( self, File file ) -> Void`

Construct a BufferedReader wrapping a File handle with default buffer size (4096 bytes). The reader does not own the file descriptor.

**Parameters**:

- `file` (`File`)
- `The File to read from.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function BufferedReader( self, File file, I64 bufferSize ) -> Void`

Construct a BufferedReader wrapping a File handle with a specified buffer size. The reader does not own the file descriptor.

**Parameters**:

- `file` (`File`)
- `The File to read from.`
- `bufferSize` (`I64`)
- `The size in bytes of the internal read-ahead buffer.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function BufferedReader( self, I64 fd, I64 bufferSize, Boolean ownsFd ) -> Void`

Construct a new BufferedReader wrapping the given file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to read from.`
- `bufferSize` (`I64`)
- `The size in bytes of the internal read-ahead buffer.`
- `ownsFd` (`Boolean`)
- `Whether this reader owns the file descriptor. If True, the`
- `file descriptor will be closed when this reader is closed.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fillBuffer( self ) -> Void`

#### `function read( self ) -> I64`

Read a single byte from the stream. Returns the byte value as an I64 in the range 0 to 255, or -1 if the end of stream has been reached. Automatically refills the internal buffer when exhausted.

**Returns**: — The next byte value, or -1 at end of stream.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readInto( self, Memory<I64> dest, I64 offset, I64 length ) -> I64`

Read up to the specified number of bytes into a destination Memory buffer. May return fewer bytes than requested if the end of stream is reached.

**Parameters**:

- `dest` (`Memory<I64>`)
- `The destination buffer to read bytes into.`
- `offset` (`I64`)
- `The starting offset within the destination buffer.`
- `length` (`I64`)
- `The maximum number of bytes to read.`

**Returns**: — The actual number of bytes read, which may be less than length at
end of stream.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function readLine( self ) -> String`

Read a single line of text from the stream, terminated by a newline character (byte value 10). The trailing newline is not included in the returned string. Carriage return characters (byte value 13) immediately before the newline are also stripped. Returns an empty string if the stream is at end of file with no remaining data.

The line buffer grows dynamically by doubling its capacity when needed.

**Returns**: — The next line of text as a null-terminated String, or empty string
at end of stream.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function isEof( self ) -> Boolean`

Check whether the end of the underlying stream has been reached. If the buffer still contains unread data, returns False. Otherwise attempts to refill the buffer to determine end-of-stream status.

**Returns**: — True if no more data is available, False otherwise.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function close( self ) -> Void`

Close this reader, freeing the internal buffer. If this reader owns the file descriptor, the descriptor is also closed. Subsequent calls after the first close are no-ops.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Destructor that delegates to close to ensure resource cleanup.

