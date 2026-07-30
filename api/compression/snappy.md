# uranite.compression.snappy

## Table of Contents

- [Imports](#imports)
- [const `SNAPPY_LITERAL`](#const-snappy-literal)
- [const `SNAPPY_COPY_1`](#const-snappy-copy-1)
- [const `SNAPPY_COPY_2`](#const-snappy-copy-2)
- [const `SNAPPY_COPY_4`](#const-snappy-copy-4)
- [const `HASH_TABLE_BITS`](#const-hash-table-bits)
- [const `HASH_TABLE_SIZE`](#const-hash-table-size)
- [const `MAX_HASH_TABLE_SIZE`](#const-max-hash-table-size)
- [const `INPUT_MARGIN_BYTES`](#const-input-margin-bytes)
- [const `MAX_BLOCK_SIZE`](#const-max-block-size)
- [const `XERIAL_MAGIC_BYTE_0`](#const-xerial-magic-byte-0)
- [const `XERIAL_MAGIC_BYTE_1`](#const-xerial-magic-byte-1)
- [const `XERIAL_MAGIC_BYTE_2`](#const-xerial-magic-byte-2)
- [const `XERIAL_MAGIC_BYTE_3`](#const-xerial-magic-byte-3)
- [const `XERIAL_MAGIC_BYTE_4`](#const-xerial-magic-byte-4)
- [const `XERIAL_MAGIC_BYTE_5`](#const-xerial-magic-byte-5)
- [const `XERIAL_MAGIC_BYTE_6`](#const-xerial-magic-byte-6)
- [const `XERIAL_MAGIC_BYTE_7`](#const-xerial-magic-byte-7)
- [const `XERIAL_HEADER_SIZE`](#const-xerial-header-size)
- [const `XERIAL_DEFAULT_BLOCK_SIZE`](#const-xerial-default-block-size)
- [function `readVarint`](#function-readvarint)
  - [`readVarint()`](#readVarint)
- [function `writeVarint`](#function-writevarint)
  - [`writeVarint()`](#writeVarint)
- [function `hashBytes`](#function-hashbytes)
  - [`hashBytes()`](#hashBytes)
- [function `load32`](#function-load32)
  - [`load32()`](#load32)
- [function `snappyMaxCompressedLength`](#function-snappymaxcompressedlength)
  - [`snappyMaxCompressedLength()`](#snappyMaxCompressedLength)
- [function `emitLiteral`](#function-emitliteral)
  - [`emitLiteral()`](#emitLiteral)
- [function `emitCopy`](#function-emitcopy)
  - [`emitCopy()`](#emitCopy)
- [function `snappyCompress`](#function-snappycompress)
  - [`snappyCompress()`](#snappyCompress)
- [function `snappyDecompress`](#function-snappydecompress)
  - [`snappyDecompress()`](#snappyDecompress)
- [function `snappyXerialCompress`](#function-snappyxerialcompress)
  - [`snappyXerialCompress()`](#snappyXerialCompress)
- [function `snappyXerialDecompress`](#function-snappyxerialdecompress)
  - [`snappyXerialDecompress()`](#snappyXerialDecompress)

## Imports

- `uranite.encoding.binary`
  - `copyBytes`
- `uranite.io.syscall`
  - `readByteAt`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `SNAPPY_LITERAL`

## const `SNAPPY_COPY_1`

## const `SNAPPY_COPY_2`

## const `SNAPPY_COPY_4`

## const `HASH_TABLE_BITS`

## const `HASH_TABLE_SIZE`

## const `MAX_HASH_TABLE_SIZE`

## const `INPUT_MARGIN_BYTES`

## const `MAX_BLOCK_SIZE`

## const `XERIAL_MAGIC_BYTE_0`

## const `XERIAL_MAGIC_BYTE_1`

## const `XERIAL_MAGIC_BYTE_2`

## const `XERIAL_MAGIC_BYTE_3`

## const `XERIAL_MAGIC_BYTE_4`

## const `XERIAL_MAGIC_BYTE_5`

## const `XERIAL_MAGIC_BYTE_6`

## const `XERIAL_MAGIC_BYTE_7`

## const `XERIAL_HEADER_SIZE`

## const `XERIAL_DEFAULT_BLOCK_SIZE`

## function `readVarint`

Read a varint-encoded unsigned integer from source buffer. Returns the decoded value in bits [0..31] and the number of bytes consumed in bits [32..63].

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address of the buffer.`
- `sourceOffset` (`I64`)
- `Offset to start reading.`

**Returns**: — I64:
Lower 32 bits = decoded value, upper 32 bits = bytes consumed.

**Complexity**:
- Time: `O(1), max 5 bytes`
- Space: `O(1)`

### Methods

#### `function readVarint( I64 sourceAddress, I64 sourceOffset ) -> I64`

Read a varint-encoded unsigned integer from source buffer. Returns the decoded value in bits [0..31] and the number of bytes consumed in bits [32..63].

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address of the buffer.`
- `sourceOffset` (`I64`)
- `Offset to start reading.`

**Returns**: — I64:
Lower 32 bits = decoded value, upper 32 bits = bytes consumed.

**Complexity**:
- Time: `O(1), max 5 bytes`
- Space: `O(1)`

## function `writeVarint`

Write a varint-encoded unsigned integer to destination buffer.

**Parameters**:

- `destAddress` (`I64`)
- `Destination base address.`
- `destOffset` (`I64`)
- `Offset to write at.`
- `value` (`I64`)
- `Value to encode` (`unsigned, max 32-bit range`)

**Returns**: — I64:
Number of bytes written.

**Complexity**:
- Time: `O(1), max 5 bytes`
- Space: `O(1)`

### Methods

#### `function writeVarint( I64 destAddress, I64 destOffset, I64 value ) -> I64`

Write a varint-encoded unsigned integer to destination buffer.

**Parameters**:

- `destAddress` (`I64`)
- `Destination base address.`
- `destOffset` (`I64`)
- `Offset to write at.`
- `value` (`I64`)
- `Value to encode` (`unsigned, max 32-bit range`)

**Returns**: — I64:
Number of bytes written.

**Complexity**:
- Time: `O(1), max 5 bytes`
- Space: `O(1)`

## function `hashBytes`

Hash 4 bytes for the Snappy hash table lookup.

**Parameters**:

- `value` (`I64`)
- `4-byte value` (`little-endian word`)
- `shift` (`I64`)
- `Right-shift amount` (`32 - table bits`)

**Returns**: — I64:
Hash table index.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function hashBytes( I64 value, I64 shift ) -> I64`

Hash 4 bytes for the Snappy hash table lookup.

**Parameters**:

- `value` (`I64`)
- `4-byte value` (`little-endian word`)
- `shift` (`I64`)
- `Right-shift amount` (`32 - table bits`)

**Returns**: — I64:
Hash table index.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `load32`

Load 4 bytes as a little-endian 32-bit value.

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address.`
- `offset` (`I64`)
- `Byte offset.`

**Returns**: — I64:
32-bit value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function load32( I64 sourceAddress, I64 offset ) -> I64`

Load 4 bytes as a little-endian 32-bit value.

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address.`
- `offset` (`I64`)
- `Byte offset.`

**Returns**: — I64:
32-bit value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `snappyMaxCompressedLength`

Maximum possible compressed size for a given input length.

**Parameters**:

- `sourceLength` (`I64`)
- `Uncompressed input length.`

**Returns**: — I64:
Worst-case compressed size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function snappyMaxCompressedLength( I64 sourceLength ) -> I64`

Maximum possible compressed size for a given input length.

**Parameters**:

- `sourceLength` (`I64`)
- `Uncompressed input length.`

**Returns**: — I64:
Worst-case compressed size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `emitLiteral`

Emit a literal element into the compressed stream.

**Parameters**:

- `destAddress` (`I64`)
- `Destination buffer address.`
- `destOffset` (`I64`)
- `Current write offset in destination.`
- `sourceAddress` (`I64`)
- `Source buffer address.`
- `literalOffset` (`I64`)
- `Start offset of the literal in source.`
- `literalLength` (`I64`)
- `Number of literal bytes.`

**Returns**: — I64:
Number of bytes written to destination.

**Complexity**:
- Time: `O(n) where n is literalLength`
- Space: `O(1)`

### Methods

#### `function emitLiteral( I64 destAddress, I64 destOffset, I64 sourceAddress, I64 literalOffset, I64 literalLength ) -> I64`

Emit a literal element into the compressed stream.

**Parameters**:

- `destAddress` (`I64`)
- `Destination buffer address.`
- `destOffset` (`I64`)
- `Current write offset in destination.`
- `sourceAddress` (`I64`)
- `Source buffer address.`
- `literalOffset` (`I64`)
- `Start offset of the literal in source.`
- `literalLength` (`I64`)
- `Number of literal bytes.`

**Returns**: — I64:
Number of bytes written to destination.

**Complexity**:
- Time: `O(n) where n is literalLength`
- Space: `O(1)`

## function `emitCopy`

Emit a copy element referencing earlier data.

**Parameters**:

- `destAddress` (`I64`)
- `Destination buffer address.`
- `destOffset` (`I64`)
- `Current write offset.`
- `matchOffset` (`I64`)
- `Backward offset to the match.`
- `matchLength` (`I64`)
- `Length of the match` (`4..64`)

**Returns**: — I64:
Number of bytes written.

**Complexity**:
- Time: `O(1) per copy element, O(n/64) for long matches`
- Space: `O(1)`

### Methods

#### `function emitCopy( I64 destAddress, I64 destOffset, I64 matchOffset, I64 matchLength ) -> I64`

Emit a copy element referencing earlier data.

**Parameters**:

- `destAddress` (`I64`)
- `Destination buffer address.`
- `destOffset` (`I64`)
- `Current write offset.`
- `matchOffset` (`I64`)
- `Backward offset to the match.`
- `matchLength` (`I64`)
- `Length of the match` (`4..64`)

**Returns**: — I64:
Number of bytes written.

**Complexity**:
- Time: `O(1) per copy element, O(n/64) for long matches`
- Space: `O(1)`

## function `snappyCompress`

Compress data using raw Snappy block format.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of uncompressed input.`
- `inputLength` (`I64`)
- `Length of input data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Compressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(HASH_TABLE_SIZE) for hash table`

### Methods

#### `function snappyCompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Compress data using raw Snappy block format.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of uncompressed input.`
- `inputLength` (`I64`)
- `Length of input data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Compressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(HASH_TABLE_SIZE) for hash table`

## function `snappyDecompress`

Decompress raw Snappy block format data.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed input.`
- `inputLength` (`I64`)
- `Length of compressed data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is compressed size`
- Space: `O(1) beyond output buffer`

### Methods

#### `function snappyDecompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Decompress raw Snappy block format data.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed input.`
- `inputLength` (`I64`)
- `Length of compressed data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is compressed size`
- Space: `O(1) beyond output buffer`

## function `snappyXerialCompress`

Compress data using the Xerial Snappy framing format used by Kafka. Prepends magic header, then chunks input into blocks each preceded by a big-endian 32-bit compressed-block length.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of uncompressed input.`
- `inputLength` (`I64`)
- `Length of input data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Total framed compressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(block size) for temporary block buffer`

### Methods

#### `function snappyXerialCompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Compress data using the Xerial Snappy framing format used by Kafka. Prepends magic header, then chunks input into blocks each preceded by a big-endian 32-bit compressed-block length.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of uncompressed input.`
- `inputLength` (`I64`)
- `Length of input data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Total framed compressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(block size) for temporary block buffer`

## function `snappyXerialDecompress`

Decompress Xerial Snappy framed data. Verifies the magic header, then decompresses each block sequentially.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed input.`
- `inputLength` (`I64`)
- `Length of compressed data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Total decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is total decompressed size`
- Space: `O(1) beyond output buffer`

### Methods

#### `function snappyXerialDecompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Decompress Xerial Snappy framed data. Verifies the magic header, then decompresses each block sequentially.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed input.`
- `inputLength` (`I64`)
- `Length of compressed data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Total decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is total decompressed size`
- Space: `O(1) beyond output buffer`

