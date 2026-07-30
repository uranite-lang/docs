# uranite.io.buffered-writer

## Table of Contents

- [Imports](#imports)
- [class `BufferedWriter`](#class-bufferedwriter)
  - [`BufferedWriter()`](#BufferedWriter)
  - [`BufferedWriter()`](#BufferedWriter)
  - [`writeByte()`](#writeByte)
  - [`writeFrom()`](#writeFrom)
  - [`writeString()`](#writeString)
  - [`flush()`](#flush)
  - [`close()`](#close)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.file`
  - `File`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `sysClose`
  - `sysFsync`
  - `sysWrite`
  - `writeByteAt`
- `uranite.memory.memory`
  - `Memory`

## class `BufferedWriter`

Buffered writer that wraps a raw file descriptor and accumulates write operations in an internal buffer. The buffer is flushed to the underlying file descriptor when full or when flush is called explicitly. Uses raw sysWrite syscalls for output.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fd` | `I64` | protect |
| `rawBuffer` | `Memory<I64>` | protect |
| `rawAddr` | `I64` | protect |
| `bufferCapacity` | `I64` | protect |
| `writePosition` | `I64` | protect |
| `closed` | `Boolean` | protect |
| `ownsFd` | `Boolean` | protect |

### Methods

#### `function BufferedWriter( self, File file ) -> Void`

Construct a BufferedWriter wrapping a File handle with default buffer size (4096 bytes). The writer does not own the file descriptor.

**Parameters**:

- `file` (`File`)
- `The File to write to.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function BufferedWriter( self, File file, I64 bufferSize ) -> Void`

Construct a BufferedWriter wrapping a File handle with a specified buffer size. The writer does not own the file descriptor.

**Parameters**:

- `file` (`File`)
- `The File to write to.`
- `bufferSize` (`I64`)
- `The size in bytes of the internal write buffer.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function BufferedWriter( self, I64 fd, I64 bufferSize, Boolean ownsFd ) -> Void`

Construct a new BufferedWriter wrapping the given file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to write to.`
- `bufferSize` (`I64`)
- `The size in bytes of the internal write buffer.`
- `ownsFd` (`Boolean`)
- `Whether this writer owns the file descriptor. If True, the`
- `file descriptor will be closed when this writer is closed.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeByte( self, I64 byteValue ) -> Void`

Write a single byte to the internal buffer. Automatically flushes the buffer to the file descriptor when the buffer becomes full.

**Parameters**:

- `byteValue` (`I64`)
- `The byte value to write, in the range 0 to 255.`

#### `function writeFrom( self, Memory<I64> data, I64 offset, I64 length ) -> Void`

Write bytes from a Memory buffer into this writer. Each byte is individually buffered and may trigger automatic flushes.

**Parameters**:

- `data` (`Memory<I64>`)
- `The source buffer containing bytes to write.`
- `offset` (`I64`)
- `The starting offset within the source buffer.`
- `length` (`I64`)
- `The number of bytes to write from the source buffer.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function writeString( self, String text ) -> Void`

Write all bytes of a string to this writer. Each byte is individually buffered and may trigger automatic flushes.

**Parameters**:

- `text` (`String`)
- `The string whose bytes will be written.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function flush( self ) -> Void`

Flush all buffered data to the underlying file descriptor. Handles partial writes by retrying until all buffered bytes are written. Resets the write position to zero after flushing. Also calls fsync to ensure data is persisted to storage.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function close( self ) -> Void`

Close this writer, flushing any remaining buffered data and freeing the internal buffer. If this writer owns the file descriptor, the descriptor is also closed. Subsequent calls after the first close are no-ops.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Destructor that delegates to close to ensure resource cleanup.

