# uranite.compression.lz4

## Table of Contents

- [Imports](#imports)
- [const `LZ4_HASH_BITS`](#const-lz4-hash-bits)
- [const `LZ4_HASH_TABLE_SIZE`](#const-lz4-hash-table-size)
- [const `LZ4_MIN_MATCH`](#const-lz4-min-match)
- [const `LZ4_LAST_LITERALS`](#const-lz4-last-literals)
- [const `LZ4_MF_LIMIT`](#const-lz4-mf-limit)
- [const `LZ4_MAX_INPUT_SIZE`](#const-lz4-max-input-size)
- [const `LZ4_FRAME_MAGIC`](#const-lz4-frame-magic)
- [const `LZ4_FRAME_VERSION`](#const-lz4-frame-version)
- [const `LZ4_BLOCK_MAX_64KB`](#const-lz4-block-max-64kb)
- [const `LZ4_BLOCK_MAX_256KB`](#const-lz4-block-max-256kb)
- [const `LZ4_BLOCK_MAX_1MB`](#const-lz4-block-max-1mb)
- [const `LZ4_BLOCK_MAX_4MB`](#const-lz4-block-max-4mb)
- [function `lz4HashPosition`](#function-lz4hashposition)
  - [`lz4HashPosition()`](#lz4HashPosition)
- [function `lz4WriteLength`](#function-lz4writelength)
  - [`lz4WriteLength()`](#lz4WriteLength)
- [function `lz4MaxCompressedSize`](#function-lz4maxcompressedsize)
  - [`lz4MaxCompressedSize()`](#lz4MaxCompressedSize)
- [function `lz4CompressBlock`](#function-lz4compressblock)
  - [`lz4CompressBlock()`](#lz4CompressBlock)
- [function `lz4DecompressBlock`](#function-lz4decompressblock)
  - [`lz4DecompressBlock()`](#lz4DecompressBlock)
- [function `lz4FrameCompress`](#function-lz4framecompress)
  - [`lz4FrameCompress()`](#lz4FrameCompress)
- [function `lz4FrameDecompress`](#function-lz4framedecompress)
  - [`lz4FrameDecompress()`](#lz4FrameDecompress)

## Imports

- `uranite.encoding.binary`
  - `copyBytes`
- `uranite.io.syscall`
  - `readByteAt`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `LZ4_HASH_BITS`

## const `LZ4_HASH_TABLE_SIZE`

## const `LZ4_MIN_MATCH`

## const `LZ4_LAST_LITERALS`

## const `LZ4_MF_LIMIT`

## const `LZ4_MAX_INPUT_SIZE`

## const `LZ4_FRAME_MAGIC`

## const `LZ4_FRAME_VERSION`

## const `LZ4_BLOCK_MAX_64KB`

## const `LZ4_BLOCK_MAX_256KB`

## const `LZ4_BLOCK_MAX_1MB`

## const `LZ4_BLOCK_MAX_4MB`

## function `lz4HashPosition`

Hash 4 bytes at position for hash table lookup.

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address.`
- `position` (`I64`)
- `Byte offset.`

**Returns**: — I64:
Hash table index.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function lz4HashPosition( I64 sourceAddress, I64 position ) -> I64`

Hash 4 bytes at position for hash table lookup.

**Parameters**:

- `sourceAddress` (`I64`)
- `Base address.`
- `position` (`I64`)
- `Byte offset.`

**Returns**: — I64:
Hash table index.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `lz4WriteLength`

Write an LZ4 extra length encoding (runs of 255 then remainder).

**Parameters**:

- `destAddress` (`I64`)
- `Destination buffer.`
- `destOffset` (`I64`)
- `Write offset.`
- `length` (`I64`)
- `Length to encode.`

**Returns**: — I64:
Bytes written.

**Complexity**:
- Time: `O(n/255)`
- Space: `O(1)`

### Methods

#### `function lz4WriteLength( I64 destAddress, I64 destOffset, I64 length ) -> I64`

Write an LZ4 extra length encoding (runs of 255 then remainder).

**Parameters**:

- `destAddress` (`I64`)
- `Destination buffer.`
- `destOffset` (`I64`)
- `Write offset.`
- `length` (`I64`)
- `Length to encode.`

**Returns**: — I64:
Bytes written.

**Complexity**:
- Time: `O(n/255)`
- Space: `O(1)`

## function `lz4MaxCompressedSize`

Worst-case compressed size for LZ4 block format.

**Parameters**:

- `inputLength` (`I64`)
- `Uncompressed input length.`

**Returns**: — I64:
Maximum compressed size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function lz4MaxCompressedSize( I64 inputLength ) -> I64`

Worst-case compressed size for LZ4 block format.

**Parameters**:

- `inputLength` (`I64`)
- `Uncompressed input length.`

**Returns**: — I64:
Maximum compressed size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `lz4CompressBlock`

Compress a single block using LZ4 block format.

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
Compressed block size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(HASH_TABLE_SIZE) for hash table`

### Methods

#### `function lz4CompressBlock( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Compress a single block using LZ4 block format.

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
Compressed block size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(HASH_TABLE_SIZE) for hash table`

## function `lz4DecompressBlock`

Decompress a single LZ4 block.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed block.`
- `inputLength` (`I64`)
- `Length of compressed data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is decompressed size`
- Space: `O(1) beyond output buffer`

### Methods

#### `function lz4DecompressBlock( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Decompress a single LZ4 block.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed block.`
- `inputLength` (`I64`)
- `Length of compressed data.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is decompressed size`
- Space: `O(1) beyond output buffer`

## function `lz4FrameCompress`

Compress data with LZ4 frame format header. Writes magic number, frame descriptor, block(s), and end mark.

**Parameters**:

- `inputAddress` (`I64`)
- `Input data address.`
- `inputLength` (`I64`)
- `Input data length.`
- `outputAddress` (`I64`)
- `Output buffer address.`
- `outputCapacity` (`I64`)
- `Output buffer capacity.`

**Returns**: — I64:
Total frame size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(block buffer)`

### Methods

#### `function lz4FrameCompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Compress data with LZ4 frame format header. Writes magic number, frame descriptor, block(s), and end mark.

**Parameters**:

- `inputAddress` (`I64`)
- `Input data address.`
- `inputLength` (`I64`)
- `Input data length.`
- `outputAddress` (`I64`)
- `Output buffer address.`
- `outputCapacity` (`I64`)
- `Output buffer capacity.`

**Returns**: — I64:
Total frame size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(block buffer)`

## function `lz4FrameDecompress`

Decompress LZ4 frame format data. Reads magic, descriptor, block(s), and end mark.

**Parameters**:

- `inputAddress` (`I64`)
- `Compressed input address.`
- `inputLength` (`I64`)
- `Compressed input length.`
- `outputAddress` (`I64`)
- `Output buffer address.`
- `outputCapacity` (`I64`)
- `Output buffer capacity.`

**Returns**: — I64:
Total decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is decompressed size`
- Space: `O(1) beyond output buffer`

### Methods

#### `function lz4FrameDecompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Decompress LZ4 frame format data. Reads magic, descriptor, block(s), and end mark.

**Parameters**:

- `inputAddress` (`I64`)
- `Compressed input address.`
- `inputLength` (`I64`)
- `Compressed input length.`
- `outputAddress` (`I64`)
- `Output buffer address.`
- `outputCapacity` (`I64`)
- `Output buffer capacity.`

**Returns**: — I64:
Total decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is decompressed size`
- Space: `O(1) beyond output buffer`

