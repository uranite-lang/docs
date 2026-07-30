# uranite.io.binary-reader

## Table of Contents

- [Imports](#imports)
- [class `BinaryReader`](#class-binaryreader)
  - [`BinaryReader()`](#BinaryReader)
  - [`readI8()`](#readI8)
  - [`readU8()`](#readU8)
  - [`readI16()`](#readI16)
  - [`readU16()`](#readU16)
  - [`readI32()`](#readI32)
  - [`readU32()`](#readU32)
  - [`readI64()`](#readI64)
  - [`readBytes()`](#readBytes)
  - [`close()`](#close)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.errors`
  - `IOError`
- `uranite.io.file`
  - `File`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readByteAt`
  - `sysClose`
  - `sysRead`
- `uranite.memory.memory`
  - `Memory`

## class `BinaryReader`

Binary reader for deserializing typed integer values from a file descriptor. Reads multi-byte integers in little-endian byte order (native x86_64 layout). Uses a single reusable temporary buffer for all read operations.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fd` | `I64` | protect |
| `tmpBuf` | `Memory<I64>` | protect |
| `tmpAddr` | `I64` | protect |
| `closed` | `Boolean` | protect |
| `ownsFd` | `Boolean` | protect |

### Methods

#### `function BinaryReader( self, File file ) -> Void`

Construct a BinaryReader wrapping a File handle. The reader does not own the file descriptor — the File handle retains ownership.

**Parameters**:

- `file` (`File`)
- `The File to read binary data from.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function BinaryReader( self, I64 fd, Boolean ownsFd ) -> Void`

Construct a new BinaryReader wrapping the given file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to read binary data from.`
- `ownsFd` (`Boolean`)
- `Whether this reader owns the file descriptor. If True, the`
- `file descriptor will be closed when this reader is closed.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function readExact( self, I64 count ) -> Void`

#### `function readI8( self ) -> I64`

Read a signed 8-bit integer from the stream. Values in the range 128-255 are sign-extended to negative values.

**Returns**: — The signed 8-bit value as an I64 in the range -128 to 127.

**Raises**:

- `IOError` → `Error` — If the stream ends before the byte can be read.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readU8( self ) -> I64`

Read an unsigned 8-bit integer from the stream.

**Returns**: — The unsigned 8-bit value as an I64 in the range 0 to 255.

**Raises**:

- `IOError` → `Error` — If the stream ends before the byte can be read.

#### `function readI16( self ) -> I64`

Read a signed 16-bit integer from the stream in little-endian byte order. Values in the range 32768-65535 are sign-extended to negative values.

**Returns**: — The signed 16-bit value as an I64 in the range -32768 to 32767.

**Raises**:

- `IOError` → `Error` — If the stream ends before both bytes can be read.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readU16( self ) -> I64`

Read an unsigned 16-bit integer from the stream in little-endian byte order.

**Returns**: — The unsigned 16-bit value as an I64 in the range 0 to 65535.

**Raises**:

- `IOError` → `Error` — If the stream ends before both bytes can be read.

#### `function readI32( self ) -> I64`

Read a signed 32-bit integer from the stream in little-endian byte order. Values in the range 2147483648-4294967295 are sign-extended to negative values.

**Returns**: — The signed 32-bit value as an I64 in the range -2147483648 to 2147483647.

**Raises**:

- `IOError` → `Error` — If the stream ends before all four bytes can be read.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readU32( self ) -> I64`

Read an unsigned 32-bit integer from the stream in little-endian byte order.

**Returns**: — The unsigned 32-bit value as an I64 in the range 0 to 4294967295.

**Raises**:

- `IOError` → `Error` — If the stream ends before all four bytes can be read.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readI64( self ) -> I64`

Read a signed 64-bit integer from the stream in little-endian byte order. Reconstructs the value from eight bytes as two 32-bit halves.

**Returns**: — The signed 64-bit value as an I64.

**Raises**:

- `IOError` → `Error` — If the stream ends before all eight bytes can be read.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readBytes( self, I64 count ) -> Memory<I64>`

Read exactly the specified number of raw bytes from the stream into a newly allocated Memory buffer. Handles partial reads by retrying.

**Parameters**:

- `count` (`I64`)
- `The exact number of bytes to read.`

**Returns**: — A newly allocated Memory buffer containing the raw bytes.

**Raises**:

- `IOError` → `Error` — If the stream ends before all bytes can be read.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function close( self ) -> Void`

Close this reader, freeing the temporary buffer. If this reader owns the file descriptor, the descriptor is also closed. Subsequent calls after the first close are no-ops.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Destructor that delegates to close to ensure resource cleanup.

