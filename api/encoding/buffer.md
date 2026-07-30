# uranite.encoding.buffer

## Table of Contents

- [Imports](#imports)
- [const `BUFFER_DEFAULT_CAPACITY`](#const-buffer-default-capacity)
- [const `BUFFER_GROWTH_FACTOR`](#const-buffer-growth-factor)
- [class `BufferOverflowError`](#class-bufferoverflowerror)
  - [`BufferOverflowError()`](#BufferOverflowError)
- [class `BinaryBuffer`](#class-binarybuffer)
  - [`BinaryBuffer()`](#BinaryBuffer)
  - [`ensureCapacity()`](#ensureCapacity)
  - [`putI8()`](#putI8)
  - [`putI16()`](#putI16)
  - [`putI32()`](#putI32)
  - [`putI64()`](#putI64)
  - [`putBytes()`](#putBytes)
  - [`putByte()`](#putByte)
  - [`putVarI32()`](#putVarI32)
  - [`putVarI64()`](#putVarI64)
  - [`putUnsignedVarI32()`](#putUnsignedVarI32)
  - [`putI32At()`](#putI32At)
  - [`putI16At()`](#putI16At)
  - [`getI8()`](#getI8)
  - [`getI16()`](#getI16)
  - [`getI32()`](#getI32)
  - [`getI64()`](#getI64)
  - [`getBytes()`](#getBytes)
  - [`getVarI32()`](#getVarI32)
  - [`getVarI64()`](#getVarI64)
  - [`getUnsignedVarI32()`](#getUnsignedVarI32)
  - [`flip()`](#flip)
  - [`reset()`](#reset)
  - [`position()`](#position)
  - [`readPos()`](#readPos)
  - [`remaining()`](#remaining)
  - [`capacity()`](#capacity)
  - [`limit()`](#limit)
  - [`address()`](#address)
  - [`setWritePosition()`](#setWritePosition)
  - [`setReadPosition()`](#setReadPosition)
  - [`setLimit()`](#setLimit)
  - [`advanceWritePosition()`](#advanceWritePosition)
  - [`flipForReading()`](#flipForReading)
  - [`destroy()`](#destroy)

## Imports

- `uranite.encoding.binary`
  - `copyBytes`
  - `readI16Be`
  - `readI32Be`
  - `readI64Be`
  - `readI8`
  - `readUnsignedVarI32`
  - `readVarI32`
  - `readVarI64`
  - `readVarI64WithLength`
  - `writeI16Be`
  - `writeI32Be`
  - `writeI64Be`
  - `writeI8`
  - `writeUnsignedVarI32`
  - `writeVarI32`
  - `writeVarI64`
- `uranite.errors.error`
  - `Error`
- `uranite.io.syscall`
  - `readByteAt`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`
  - `realloc`

## const `BUFFER_DEFAULT_CAPACITY`

Default initial capacity in bytes for new buffers.

## const `BUFFER_GROWTH_FACTOR`

Factor by which buffer capacity is multiplied when it needs to grow.

## class `BufferOverflowError`

**Extends**: `Error`

Raised when a read operation attempts to access bytes beyond the buffer's current limit.

### Methods

#### `function BufferOverflowError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a buffer overflow error.

**Parameters**:

- `message` (`String`)
- `Description of the overflow condition.`
- `code` (`I64`)
- `Numeric error code.`
- `cause` (`?Error`)
- `Optional underlying error.`

## class `BinaryBuffer`

Growable binary buffer for constructing and parsing wire protocol messages. Maintains a write position for appending data and a separate read position for sequential parsing. The buffer automatically doubles its backing allocation when a write would exceed capacity.

After writing, call flip() to prepare for reading: this sets the limit to the current write position and resets the read position to zero.

### Fields

| Name | Type | Access |
|------|------|--------|
| `dataAddress` | `I64` | protect |
| `bufferCapacity` | `I64` | protect |
| `writePosition` | `I64` | protect |
| `readPosition` | `I64` | protect |
| `bufferLimit` | `I64` | protect |

### Methods

#### `function BinaryBuffer( self, I64 initialCapacity ) -> Void`

Construct a new binary buffer with the specified initial capacity.

**Parameters**:

- `initialCapacity` (`I64`)
- `Initial allocation size in bytes. If zero or negative,`
- `BUFFER_DEFAULT_CAPACITY` (`1024`)

**Complexity**:
- Time: `O(1)`
- Space: `O(n) where n is initialCapacity`

#### `function ensureCapacity( self, I64 additionalBytes ) -> Void`

Ensure the buffer has room for at least additionalBytes more bytes at the current write position. Doubles capacity until sufficient.

**Parameters**:

- `additionalBytes` (`I64`)
- `Number of additional bytes needed.`

**Complexity**:
- Time: `O(n) amortized where n is current data size (on realloc)`
- Space: `O(n) where n is new capacity (on realloc)`

#### `function putI8( self, I64 value ) -> Void`

Write a single byte at the current write position and advance it.

**Parameters**:

- `value` (`I64`)
- `The value whose low 8 bits will be written.`

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function putI16( self, I64 value ) -> Void`

Write a 16-bit big-endian integer at the current write position.

**Parameters**:

- `value` (`I64`)
- `The value whose low 16 bits will be written.`

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function putI32( self, I64 value ) -> Void`

Write a 32-bit big-endian integer at the current write position.

**Parameters**:

- `value` (`I64`)
- `The value whose low 32 bits will be written.`

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function putI64( self, I64 value ) -> Void`

Write a 64-bit big-endian integer at the current write position.

**Parameters**:

- `value` (`I64`)
- `The 64-bit value to write.`

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function putBytes( self, I64 sourceAddress, I64 sourceLength ) -> Void`

Write raw bytes from source into the buffer at the current write position.

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address of the source data.`
- `sourceLength` (`I64`)
- `Number of bytes to copy.`

**Complexity**:
- Time: `O(n) where n is sourceLength`
- Space: `O(1)`

#### `function putByte( self, I64 value ) -> Void`

Write a single byte value at the current write position. Alias for putI8 with explicit byte semantics.

**Parameters**:

- `value` (`I64`)
- `Byte value` (`0-255`)

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function putVarI32( self, I64 value ) -> Void`

Write a zig-zag encoded variable-length 32-bit integer at the current write position.

**Parameters**:

- `value` (`I64`)
- `The signed 32-bit value to encode.`

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function putVarI64( self, I64 value ) -> Void`

Write a zig-zag encoded variable-length 64-bit integer at the current write position.

**Parameters**:

- `value` (`I64`)
- `The signed 64-bit value to encode.`

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function putUnsignedVarI32( self, I64 value ) -> Void`

Write an unsigned variable-length 32-bit integer (no zig-zag) at the current write position.

**Parameters**:

- `value` (`I64`)
- `The unsigned 32-bit value to encode.`

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function putI32At( self, I64 absoluteOffset, I64 value ) -> Void`

Write a 32-bit big-endian integer at an absolute offset without advancing the write position. Used for backpatching length fields after the payload size is known.

**Parameters**:

- `absoluteOffset` (`I64`)
- `Absolute byte offset in the buffer.`
- `value` (`I64`)
- `The value whose low 32 bits will be written.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function putI16At( self, I64 absoluteOffset, I64 value ) -> Void`

Write a 16-bit big-endian integer at an absolute offset without advancing the write position.

**Parameters**:

- `absoluteOffset` (`I64`)
- `Absolute byte offset in the buffer.`
- `value` (`I64`)
- `The value whose low 16 bits will be written.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getI8( self ) -> I64`

Read a signed byte from the current read position and advance it.

**Returns**: `I64` — The signed byte value (-128 to 127).

**Raises**:

- `BufferOverflowError` → `Error` — When fewer than 1 byte remains before the limit.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getI16( self ) -> I64`

Read a signed 16-bit big-endian integer from the current read position and advance it.

**Returns**: `I64` — The signed 16-bit value.

**Raises**:

- `BufferOverflowError` → `Error` — When fewer than 2 bytes remain.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getI32( self ) -> I64`

Read a signed 32-bit big-endian integer from the current read position and advance it.

**Returns**: `I64` — The signed 32-bit value.

**Raises**:

- `BufferOverflowError` → `Error` — When fewer than 4 bytes remain.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getI64( self ) -> I64`

Read a 64-bit big-endian integer from the current read position and advance it.

**Returns**: `I64` — The 64-bit value.

**Raises**:

- `BufferOverflowError` → `Error` — When fewer than 8 bytes remain.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getBytes( self, I64 destinationAddress, I64 byteCount ) -> Void`

Read byteCount bytes from the current read position into the destination buffer and advance the read position.

**Parameters**:

- `destinationAddress` (`I64`)
- `Base address of the destination buffer.`
- `byteCount` (`I64`)
- `Number of bytes to read.`

**Raises**:

- `BufferOverflowError` → `Error` — When fewer than byteCount bytes remain.

**Complexity**:
- Time: `O(n) where n is byteCount`
- Space: `O(1)`

#### `function getVarI32( self ) -> I64`

Read a zig-zag encoded variable-length 32-bit integer from the current read position and advance it.

**Returns**: — I64:
The decoded signed 32-bit value.

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function getVarI64( self ) -> I64`

Read a zig-zag encoded variable-length 64-bit integer from the current read position and advance it.

**Returns**: — I64:
The decoded signed 64-bit value.

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function getUnsignedVarI32( self ) -> I64`

Read an unsigned variable-length 32-bit integer from the current read position and advance it.

**Returns**: — I64:
The unsigned 32-bit value.

**Complexity**:
- Time: `O(1) amortized`
- Space: `O(1)`

#### `function flip( self ) -> Void`

Prepare the buffer for reading after writing. Sets the limit to the current write position and resets the read position to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function reset( self ) -> Void`

Reset both read and write positions to zero and limit to capacity. Does not deallocate or zero the backing memory.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function position( self ) -> I64`

Return the current write position.

**Returns**: `I64` — The byte offset where the next write will occur.

#### `function readPos( self ) -> I64`

Return the current read position.

**Returns**: `I64` — The byte offset where the next read will occur.

#### `function remaining( self ) -> I64`

Return the number of bytes available for reading between the current read position and the limit.

**Returns**: `I64` — Number of readable bytes remaining.

#### `function capacity( self ) -> I64`

Return the current buffer capacity.

**Returns**: `I64` — Total allocated size in bytes.

#### `function limit( self ) -> I64`

Return the current buffer limit.

**Returns**: `I64` — The limit beyond which reads are not allowed.

#### `function address( self ) -> I64`

Return the raw memory address of the buffer's backing allocation. Use with caution — the address may change after ensureCapacity triggers a reallocation.

**Returns**: `I64` — Base address of the buffer data.

#### `function setWritePosition( self, I64 newPosition ) -> Void`

Set the write position to an absolute offset. Used for backpatching or rewinding writes.

**Parameters**:

- `newPosition` (`I64`)
- `The new write position.`

#### `function setReadPosition( self, I64 newPosition ) -> Void`

Set the read position to an absolute offset.

**Parameters**:

- `newPosition` (`I64`)
- `The new read position.`

#### `function setLimit( self, I64 newLimit ) -> Void`

Set the buffer limit explicitly.

**Parameters**:

- `newLimit` (`I64`)
- `The new limit value.`

#### `function advanceWritePosition( self, I64 byteCount ) -> Void`

Advance the write position by the given byte count without writing data. Used after raw memory copy into the buffer's backing address to synchronize the write cursor.

**Parameters**:

- `byteCount` (`I64`)
- `Number of bytes to advance.`

**Complexity**:
- Time: `O(1)`

#### `function flipForReading( self ) -> Void`

Prepare the buffer for reading from the start. Sets the buffer limit to the current write position and resets the read position to zero. After this call, remaining() returns the number of bytes written.

**Complexity**:
- Time: `O(1)`

#### `function destroy( self ) -> Void`

Deallocate the buffer's backing memory. The buffer must not be used after this call.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

