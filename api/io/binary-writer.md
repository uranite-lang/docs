# uranite.io.binary-writer

## Table of Contents

- [Imports](#imports)
- [class `BinaryWriter`](#class-binarywriter)
  - [`BinaryWriter()`](#BinaryWriter)
  - [`writeI8()`](#writeI8)
  - [`writeU8()`](#writeU8)
  - [`writeI16()`](#writeI16)
  - [`writeU16()`](#writeU16)
  - [`writeI32()`](#writeI32)
  - [`writeU32()`](#writeU32)
  - [`writeI64()`](#writeI64)
  - [`writeBytes()`](#writeBytes)
  - [`flush()`](#flush)
  - [`close()`](#close)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.file`
  - `File`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `sysClose`
  - `sysFsync`
  - `sysWrite`
  - `writeByteAt`
- `uranite.memory.memory`
  - `Memory`

## class `BinaryWriter`

Binary writer for serializing typed integer values to a file descriptor. Writes multi-byte integers in little-endian byte order (native x86_64 layout). Uses a single reusable temporary buffer for all write operations.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fd` | `I64` | protect |
| `tmpBuf` | `Memory<I64>` | protect |
| `tmpAddr` | `I64` | protect |
| `closed` | `Boolean` | protect |
| `ownsFd` | `Boolean` | protect |

### Methods

#### `function BinaryWriter( self, File file ) -> Void`

Construct a BinaryWriter wrapping a File handle. The writer does not own the file descriptor — the File handle retains ownership.

**Parameters**:

- `file` (`File`)
- `The File to write binary data to.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function BinaryWriter( self, I64 fd, Boolean ownsFd ) -> Void`

Construct a new BinaryWriter wrapping the given file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to write binary data to.`
- `ownsFd` (`Boolean`)
- `Whether this writer owns the file descriptor. If True, the`
- `file descriptor will be closed when this writer is closed.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function writeExact( self, I64 count ) -> Void`

#### `function writeI8( self, I64 value ) -> Void`

Write a signed 8-bit integer to the stream. Negative values are converted to their unsigned byte representation.

**Parameters**:

- `value` (`I64`)
- `The signed value to write, expected in the range -128 to 127.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeU8( self, I64 value ) -> Void`

Write an unsigned 8-bit integer to the stream.

**Parameters**:

- `value` (`I64`)
- `The unsigned value to write, expected in the range 0 to 255.`

#### `function writeI16( self, I64 value ) -> Void`

Write a signed 16-bit integer to the stream in little-endian byte order. Negative values are converted to their two's complement representation.

**Parameters**:

- `value` (`I64`)
- `The signed value to write, expected in the range -32768 to 32767.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeU16( self, I64 value ) -> Void`

Write an unsigned 16-bit integer to the stream in little-endian byte order.

**Parameters**:

- `value` (`I64`)
- `The unsigned value to write, expected in the range 0 to 65535.`

#### `function writeI32( self, I64 value ) -> Void`

Write a signed 32-bit integer to the stream in little-endian byte order. Negative values are converted to their two's complement representation.

**Parameters**:

- `value` (`I64`)
- `The signed value to write, expected in the range -2147483648 to 2147483647.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeU32( self, I64 value ) -> Void`

Write an unsigned 32-bit integer to the stream in little-endian byte order.

**Parameters**:

- `value` (`I64`)
- `The unsigned value to write, expected in the range 0 to 4294967295.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeI64( self, I64 value ) -> Void`

Write a signed 64-bit integer to the stream in little-endian byte order. Decomposes the value into low and high 32-bit halves before writing.

**Parameters**:

- `value` (`I64`)
- `The 64-bit value to write.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeBytes( self, Memory<I64> data, I64 length ) -> Void`

Write raw bytes from a Memory buffer directly to the file descriptor. Handles partial writes by retrying until all bytes are written.

**Parameters**:

- `data` (`Memory<I64>`)
- `The source buffer containing the raw bytes to write.`
- `length` (`I64`)
- `The number of bytes to write from the buffer.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function flush( self ) -> Void`

Flush all buffered data to the underlying storage device by calling fsync on the file descriptor.

#### `function close( self ) -> Void`

Close this writer, flushing any pending data and freeing the temporary buffer. If this writer owns the file descriptor, the descriptor is also closed. Subsequent calls after the first close are no-ops.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Destructor that delegates to close to ensure resource cleanup.

