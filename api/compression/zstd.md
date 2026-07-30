# uranite.compression.zstd

## Table of Contents

- [Imports](#imports)
- [const `ZSTD_MAGIC`](#const-zstd-magic)
- [const `ZSTD_FRAME_HEADER_MIN`](#const-zstd-frame-header-min)
- [const `ZSTD_BLOCK_RAW`](#const-zstd-block-raw)
- [const `ZSTD_BLOCK_RLE`](#const-zstd-block-rle)
- [const `ZSTD_BLOCK_COMPRESSED`](#const-zstd-block-compressed)
- [const `ZSTD_BLOCK_RESERVED`](#const-zstd-block-reserved)
- [const `ZSTD_MAX_WINDOW_SIZE`](#const-zstd-max-window-size)
- [const `ZSTD_CONTENT_CHECKSUM_SIZE`](#const-zstd-content-checksum-size)
- [const `ZSTD_LITBLOCK_RAW`](#const-zstd-litblock-raw)
- [const `ZSTD_LITBLOCK_RLE`](#const-zstd-litblock-rle)
- [const `ZSTD_LITBLOCK_COMPRESSED`](#const-zstd-litblock-compressed)
- [const `ZSTD_LITBLOCK_TREELESS`](#const-zstd-litblock-treeless)
- [const `ZSTD_SEQBLOCK_PREDEFINED`](#const-zstd-seqblock-predefined)
- [const `ZSTD_SEQBLOCK_RLE`](#const-zstd-seqblock-rle)
- [const `ZSTD_SEQBLOCK_FSE`](#const-zstd-seqblock-fse)
- [const `ZSTD_SEQBLOCK_REPEAT`](#const-zstd-seqblock-repeat)
- [const `MAX_SEQUENCES`](#const-max-sequences)
- [const `MAX_LITERALS`](#const-max-literals)
- [function `load32Le`](#function-load32le)
  - [`load32Le()`](#load32Le)
- [function `parseFrameHeader`](#function-parseframeheader)
  - [`parseFrameHeader()`](#parseFrameHeader)
- [function `zstdDecompress`](#function-zstddecompress)
  - [`zstdDecompress()`](#zstdDecompress)
- [function `zstdCompress`](#function-zstdcompress)
  - [`zstdCompress()`](#zstdCompress)
- [function `zstdMaxCompressedSize`](#function-zstdmaxcompressedsize)
  - [`zstdMaxCompressedSize()`](#zstdMaxCompressedSize)

## Imports

- `uranite.encoding.binary`
  - `copyBytes`
- `uranite.io.syscall`
  - `readByteAt`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `ZSTD_MAGIC`

## const `ZSTD_FRAME_HEADER_MIN`

## const `ZSTD_BLOCK_RAW`

## const `ZSTD_BLOCK_RLE`

## const `ZSTD_BLOCK_COMPRESSED`

## const `ZSTD_BLOCK_RESERVED`

## const `ZSTD_MAX_WINDOW_SIZE`

## const `ZSTD_CONTENT_CHECKSUM_SIZE`

## const `ZSTD_LITBLOCK_RAW`

## const `ZSTD_LITBLOCK_RLE`

## const `ZSTD_LITBLOCK_COMPRESSED`

## const `ZSTD_LITBLOCK_TREELESS`

## const `ZSTD_SEQBLOCK_PREDEFINED`

## const `ZSTD_SEQBLOCK_RLE`

## const `ZSTD_SEQBLOCK_FSE`

## const `ZSTD_SEQBLOCK_REPEAT`

## const `MAX_SEQUENCES`

## const `MAX_LITERALS`

## function `load32Le`

Load 4 bytes as little-endian 32-bit value.

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

#### `function load32Le( I64 sourceAddress, I64 offset ) -> I64`

Load 4 bytes as little-endian 32-bit value.

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

## function `parseFrameHeader`

Parse a Zstd frame header and return the content size if present, otherwise -1. Also returns the header size via upper bits.

Result encoding: bits [0..31] = content size (-1 if unknown), bits [32..39] = total header size.

**Parameters**:

- `inputAddress` (`I64`)
- `Frame start address.`
- `inputLength` (`I64`)
- `Available bytes.`

**Returns**: — I64:
Packed (headerSize << 32) | contentSize.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function parseFrameHeader( I64 inputAddress, I64 inputLength ) -> I64`

Parse a Zstd frame header and return the content size if present, otherwise -1. Also returns the header size via upper bits.

Result encoding: bits [0..31] = content size (-1 if unknown), bits [32..39] = total header size.

**Parameters**:

- `inputAddress` (`I64`)
- `Frame start address.`
- `inputLength` (`I64`)
- `Available bytes.`

**Returns**: — I64:
Packed (headerSize << 32) | contentSize.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `zstdDecompress`

Decompress a Zstd frame. Handles Raw, RLE, and Compressed block types. Compressed blocks with FSE/Huffman decoding support only raw and RLE literal sub-blocks (full FSE literal decoding deferred).

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed Zstd frame.`
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

#### `function zstdDecompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Decompress a Zstd frame. Handles Raw, RLE, and Compressed block types. Compressed blocks with FSE/Huffman decoding support only raw and RLE literal sub-blocks (full FSE literal decoding deferred).

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed Zstd frame.`
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

## function `zstdCompress`

Compress data into a valid Zstd frame using Raw blocks only. This produces valid Zstd output that any compliant decoder can read, with no entropy coding (size overhead ~3 bytes per 128KB block + frame header/trailer).

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
Compressed frame size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(1)`

### Methods

#### `function zstdCompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Compress data into a valid Zstd frame using Raw blocks only. This produces valid Zstd output that any compliant decoder can read, with no entropy coding (size overhead ~3 bytes per 128KB block + frame header/trailer).

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
Compressed frame size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(1)`

## function `zstdMaxCompressedSize`

Maximum possible frame size for raw-block compression.

**Parameters**:

- `inputLength` (`I64`)
- `Input data length.`

**Returns**: — I64:
Worst-case output size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function zstdMaxCompressedSize( I64 inputLength ) -> I64`

Maximum possible frame size for raw-block compression.

**Parameters**:

- `inputLength` (`I64`)
- `Input data length.`

**Returns**: — I64:
Worst-case output size.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

