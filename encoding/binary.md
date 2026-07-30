# uranite.encoding.binary

## Table of Contents

- [Imports](#imports)
- [function `readI8`](#function-readi8)
  - [`readI8()`](#readI8)
- [function `writeI8`](#function-writei8)
  - [`writeI8()`](#writeI8)
- [function `readI16Be`](#function-readi16be)
  - [`readI16Be()`](#readI16Be)
- [function `writeI16Be`](#function-writei16be)
  - [`writeI16Be()`](#writeI16Be)
- [function `readI32Be`](#function-readi32be)
  - [`readI32Be()`](#readI32Be)
- [function `writeI32Be`](#function-writei32be)
  - [`writeI32Be()`](#writeI32Be)
- [function `readI64Be`](#function-readi64be)
  - [`readI64Be()`](#readI64Be)
- [function `writeI64Be`](#function-writei64be)
  - [`writeI64Be()`](#writeI64Be)
- [function `readVarI32`](#function-readvari32)
  - [`readVarI32()`](#readVarI32)
- [function `writeVarI32`](#function-writevari32)
  - [`writeVarI32()`](#writeVarI32)
- [function `readVarI64`](#function-readvari64)
  - [`readVarI64()`](#readVarI64)
- [function `readVarI64WithLength`](#function-readvari64withlength)
  - [`readVarI64WithLength()`](#readVarI64WithLength)
- [function `writeVarI64`](#function-writevari64)
  - [`writeVarI64()`](#writeVarI64)
- [function `readUnsignedVarI32`](#function-readunsignedvari32)
  - [`readUnsignedVarI32()`](#readUnsignedVarI32)
- [function `writeUnsignedVarI32`](#function-writeunsignedvari32)
  - [`writeUnsignedVarI32()`](#writeUnsignedVarI32)
- [function `copyBytes`](#function-copybytes)
  - [`copyBytes()`](#copyBytes)

## Imports

- `uranite.io.syscall`
  - `readByteAt`
  - `writeByteAt`

## function `readI8`

Read a single signed byte from the buffer at the given offset and sign-extend it to I64.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`

**Returns**: — I64:
The signed byte value sign-extended to 64 bits (-128 to 127).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readI8( I64 bufferAddress, I64 offset ) -> I64`

Read a single signed byte from the buffer at the given offset and sign-extend it to I64.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`

**Returns**: — I64:
The signed byte value sign-extended to 64 bits (-128 to 127).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `writeI8`

Write the low byte of value to the buffer at the given offset.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`
- `value` (`I64`)
- `The value whose low 8 bits will be written.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function writeI8( I64 bufferAddress, I64 offset, I64 value ) -> Void`

Write the low byte of value to the buffer at the given offset.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`
- `value` (`I64`)
- `The value whose low 8 bits will be written.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readI16Be`

Read a signed 16-bit big-endian integer from the buffer at the given offset and sign-extend it to I64.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`

**Returns**: — I64:
The signed 16-bit value sign-extended to 64 bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readI16Be( I64 bufferAddress, I64 offset ) -> I64`

Read a signed 16-bit big-endian integer from the buffer at the given offset and sign-extend it to I64.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`

**Returns**: — I64:
The signed 16-bit value sign-extended to 64 bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `writeI16Be`

Write a 16-bit integer in big-endian byte order to the buffer at the given offset.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`
- `value` (`I64`)
- `The value whose low 16 bits will be written in big-endian order.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function writeI16Be( I64 bufferAddress, I64 offset, I64 value ) -> Void`

Write a 16-bit integer in big-endian byte order to the buffer at the given offset.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`
- `value` (`I64`)
- `The value whose low 16 bits will be written in big-endian order.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readI32Be`

Read a signed 32-bit big-endian integer from the buffer at the given offset and sign-extend it to I64.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`

**Returns**: — I64:
The signed 32-bit value sign-extended to 64 bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readI32Be( I64 bufferAddress, I64 offset ) -> I64`

Read a signed 32-bit big-endian integer from the buffer at the given offset and sign-extend it to I64.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`

**Returns**: — I64:
The signed 32-bit value sign-extended to 64 bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `writeI32Be`

Write a 32-bit integer in big-endian byte order to the buffer at the given offset.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`
- `value` (`I64`)
- `The value whose low 32 bits will be written in big-endian order.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function writeI32Be( I64 bufferAddress, I64 offset, I64 value ) -> Void`

Write a 32-bit integer in big-endian byte order to the buffer at the given offset.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`
- `value` (`I64`)
- `The value whose low 32 bits will be written in big-endian order.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readI64Be`

Read a 64-bit big-endian integer from the buffer at the given offset. The value is returned as a signed I64 with no additional conversion since I64 is already the native 64-bit signed type.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`

**Returns**: — I64:
The 64-bit value read in big-endian order.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readI64Be( I64 bufferAddress, I64 offset ) -> I64`

Read a 64-bit big-endian integer from the buffer at the given offset. The value is returned as a signed I64 with no additional conversion since I64 is already the native 64-bit signed type.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`

**Returns**: — I64:
The 64-bit value read in big-endian order.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `writeI64Be`

Write a 64-bit integer in big-endian byte order to the buffer at the given offset.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`
- `value` (`I64`)
- `The 64-bit value to write in big-endian order.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function writeI64Be( I64 bufferAddress, I64 offset, I64 value ) -> Void`

Write a 64-bit integer in big-endian byte order to the buffer at the given offset.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset into the buffer.`
- `value` (`I64`)
- `The 64-bit value to write in big-endian order.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readVarI32`

Read a zig-zag encoded variable-length 32-bit integer from the buffer. Returns the decoded signed value packed with the number of bytes consumed: the low 32 bits contain the decoded value, the high 32 bits contain the byte count. Extract with: value = result & 0xFFFFFFFF (sign-extend if bit 31 set), bytesRead = result >> 32.

Zig-zag encoding maps signed integers to unsigned via (n << 1) ^ (n >> 31) so small-magnitude values (positive or negative) use fewer bytes.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start reading from.`

**Returns**: — I64:
Packed result: low 32 bits = decoded signed value,
bits 32-63 = number of bytes consumed.

**Complexity**:
- Time: `O(1) amortized (max 5 bytes for 32-bit varint)`
- Space: `O(1)`

### Methods

#### `function readVarI32( I64 bufferAddress, I64 offset ) -> I64`

Read a zig-zag encoded variable-length 32-bit integer from the buffer. Returns the decoded signed value packed with the number of bytes consumed: the low 32 bits contain the decoded value, the high 32 bits contain the byte count. Extract with: value = result & 0xFFFFFFFF (sign-extend if bit 31 set), bytesRead = result >> 32.

Zig-zag encoding maps signed integers to unsigned via (n << 1) ^ (n >> 31) so small-magnitude values (positive or negative) use fewer bytes.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start reading from.`

**Returns**: — I64:
Packed result: low 32 bits = decoded signed value,
bits 32-63 = number of bytes consumed.

**Complexity**:
- Time: `O(1) amortized (max 5 bytes for 32-bit varint)`
- Space: `O(1)`

## function `writeVarI32`

Write a signed 32-bit integer as a zig-zag encoded variable-length integer to the buffer at the given offset. Returns the number of bytes written.

Zig-zag encoding: unsigned = (value << 1) ^ (value >> 31).

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start writing at.`
- `value` (`I64`)
- `The signed 32-bit value to encode.`

**Returns**: — I64:
The number of bytes written (1-5).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function writeVarI32( I64 bufferAddress, I64 offset, I64 value ) -> I64`

Write a signed 32-bit integer as a zig-zag encoded variable-length integer to the buffer at the given offset. Returns the number of bytes written.

Zig-zag encoding: unsigned = (value << 1) ^ (value >> 31).

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start writing at.`
- `value` (`I64`)
- `The signed 32-bit value to encode.`

**Returns**: — I64:
The number of bytes written (1-5).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readVarI64`

Read a zig-zag encoded variable-length 64-bit integer from the buffer. Returns the decoded signed value. Since VarI64 can consume up to 10 bytes and the full 64-bit range is needed for the value, the byte count is not packed into the return value. Use readVarI64WithLength for the packed variant.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start reading from.`

**Returns**: — I64:
The decoded signed 64-bit value.

**Complexity**:
- Time: `O(1) amortized (max 10 bytes for 64-bit varint)`
- Space: `O(1)`

### Methods

#### `function readVarI64( I64 bufferAddress, I64 offset ) -> I64`

Read a zig-zag encoded variable-length 64-bit integer from the buffer. Returns the decoded signed value. Since VarI64 can consume up to 10 bytes and the full 64-bit range is needed for the value, the byte count is not packed into the return value. Use readVarI64WithLength for the packed variant.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start reading from.`

**Returns**: — I64:
The decoded signed 64-bit value.

**Complexity**:
- Time: `O(1) amortized (max 10 bytes for 64-bit varint)`
- Space: `O(1)`

## function `readVarI64WithLength`

Read a zig-zag encoded variable-length 64-bit integer from the buffer, storing the number of bytes consumed at lengthOutputAddress.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start reading from.`
- `lengthOutputAddress` (`I64`)
- `Address where the byte count consumed will be written as I64.`

**Returns**: — I64:
The decoded signed 64-bit value.

**Complexity**:
- Time: `O(1) amortized (max 10 bytes)`
- Space: `O(1)`

### Methods

#### `function readVarI64WithLength( I64 bufferAddress, I64 offset, I64 lengthOutputAddress ) -> I64`

Read a zig-zag encoded variable-length 64-bit integer from the buffer, storing the number of bytes consumed at lengthOutputAddress.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start reading from.`
- `lengthOutputAddress` (`I64`)
- `Address where the byte count consumed will be written as I64.`

**Returns**: — I64:
The decoded signed 64-bit value.

**Complexity**:
- Time: `O(1) amortized (max 10 bytes)`
- Space: `O(1)`

## function `writeVarI64`

Write a signed 64-bit integer as a zig-zag encoded variable-length integer to the buffer at the given offset. Returns the number of bytes written.

Zig-zag encoding: unsigned = (value << 1) ^ (value >> 63).

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start writing at.`
- `value` (`I64`)
- `The signed 64-bit value to encode.`

**Returns**: — I64:
The number of bytes written (1-10).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function writeVarI64( I64 bufferAddress, I64 offset, I64 value ) -> I64`

Write a signed 64-bit integer as a zig-zag encoded variable-length integer to the buffer at the given offset. Returns the number of bytes written.

Zig-zag encoding: unsigned = (value << 1) ^ (value >> 63).

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start writing at.`
- `value` (`I64`)
- `The signed 64-bit value to encode.`

**Returns**: — I64:
The number of bytes written (1-10).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readUnsignedVarI32`

Read an unsigned variable-length 32-bit integer (no zig-zag encoding) from the buffer. Used by Kafka compact protocol for string/array lengths. Returns the value packed with byte count: low 32 bits = value, high 32 bits = bytes consumed.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start reading from.`

**Returns**: — I64:
Packed result: low 32 bits = unsigned value,
bits 32-63 = bytes consumed.

**Complexity**:
- Time: `O(1) amortized (max 5 bytes)`
- Space: `O(1)`

### Methods

#### `function readUnsignedVarI32( I64 bufferAddress, I64 offset ) -> I64`

Read an unsigned variable-length 32-bit integer (no zig-zag encoding) from the buffer. Used by Kafka compact protocol for string/array lengths. Returns the value packed with byte count: low 32 bits = value, high 32 bits = bytes consumed.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start reading from.`

**Returns**: — I64:
Packed result: low 32 bits = unsigned value,
bits 32-63 = bytes consumed.

**Complexity**:
- Time: `O(1) amortized (max 5 bytes)`
- Space: `O(1)`

## function `writeUnsignedVarI32`

Write an unsigned 32-bit integer as a variable-length integer (no zig-zag encoding) to the buffer. Used by Kafka compact protocol for string/array lengths. Returns the number of bytes written.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start writing at.`
- `value` (`I64`)
- `The unsigned 32-bit value to encode.`

**Returns**: — I64:
The number of bytes written (1-5).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function writeUnsignedVarI32( I64 bufferAddress, I64 offset, I64 value ) -> I64`

Write an unsigned 32-bit integer as a variable-length integer (no zig-zag encoding) to the buffer. Used by Kafka compact protocol for string/array lengths. Returns the number of bytes written.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the buffer.`
- `offset` (`I64`)
- `Byte offset to start writing at.`
- `value` (`I64`)
- `The unsigned 32-bit value to encode.`

**Returns**: — I64:
The number of bytes written (1-5).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `copyBytes`

Copy byteCount bytes from source buffer to destination buffer. Handles overlapping regions correctly when source is before destination by copying backward.

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address of the source buffer.`
- `sourceOffset` (`I64`)
- `Byte offset in the source buffer.`
- `destinationAddress` (`I64`)
- `Base address of the destination buffer.`
- `destinationOffset` (`I64`)
- `Byte offset in the destination buffer.`
- `byteCount` (`I64`)
- `Number of bytes to copy.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function copyBytes( I64 sourceAddress, I64 sourceOffset, I64 destinationAddress, I64 destinationOffset, I64 byteCount ) -> Void`

Copy byteCount bytes from source buffer to destination buffer. Handles overlapping regions correctly when source is before destination by copying backward.

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address of the source buffer.`
- `sourceOffset` (`I64`)
- `Byte offset in the source buffer.`
- `destinationAddress` (`I64`)
- `Base address of the destination buffer.`
- `destinationOffset` (`I64`)
- `Byte offset in the destination buffer.`
- `byteCount` (`I64`)
- `Number of bytes to copy.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

