# uranite.compression.gzip

## Table of Contents

- [Imports](#imports)
- [const `GZIP_MAGIC_1`](#const-gzip-magic-1)
- [const `GZIP_MAGIC_2`](#const-gzip-magic-2)
- [const `GZIP_METHOD_DEFLATE`](#const-gzip-method-deflate)
- [const `GZIP_HEADER_SIZE`](#const-gzip-header-size)
- [const `GZIP_TRAILER_SIZE`](#const-gzip-trailer-size)
- [const `DEFLATE_MAX_BITS`](#const-deflate-max-bits)
- [const `DEFLATE_MAX_LIT_CODES`](#const-deflate-max-lit-codes)
- [const `DEFLATE_MAX_DIST_CODES`](#const-deflate-max-dist-codes)
- [const `DEFLATE_MAX_CODES`](#const-deflate-max-codes)
- [const `DEFLATE_CODE_LENGTH_ORDER_COUNT`](#const-deflate-code-length-order-count)
- [const `DEFLATE_WINDOW_SIZE`](#const-deflate-window-size)
- [const `DEFLATE_HASH_BITS`](#const-deflate-hash-bits)
- [const `DEFLATE_HASH_SIZE`](#const-deflate-hash-size)
- [const `DEFLATE_MIN_MATCH`](#const-deflate-min-match)
- [const `DEFLATE_MAX_MATCH`](#const-deflate-max-match)
- [const `CODE_LENGTH_ORDER_ADDRESS`](#const-code-length-order-address)
- [class `BitReader`](#class-bitreader)
  - [`BitReader()`](#BitReader)
  - [`readBits()`](#readBits)
  - [`alignToByte()`](#alignToByte)
- [class `BitWriter`](#class-bitwriter)
  - [`BitWriter()`](#BitWriter)
  - [`writeBits()`](#writeBits)
  - [`flush()`](#flush)
- [class `HuffmanTable`](#class-huffmantable)
  - [`HuffmanTable()`](#HuffmanTable)
  - [`build()`](#build)
  - [`decode()`](#decode)
  - [`destroy()`](#destroy)
- [function `buildFixedLitLenTable`](#function-buildfixedlitlentable)
  - [`buildFixedLitLenTable()`](#buildFixedLitLenTable)
- [function `buildFixedDistTable`](#function-buildfixeddisttable)
  - [`buildFixedDistTable()`](#buildFixedDistTable)
- [function `getLengthBase`](#function-getlengthbase)
  - [`getLengthBase()`](#getLengthBase)
- [function `getLengthExtraBits`](#function-getlengthextrabits)
  - [`getLengthExtraBits()`](#getLengthExtraBits)
- [function `getDistBase`](#function-getdistbase)
  - [`getDistBase()`](#getDistBase)
- [function `getDistExtraBits`](#function-getdistextrabits)
  - [`getDistExtraBits()`](#getDistExtraBits)
- [function `inflate`](#function-inflate)
  - [`inflate()`](#inflate)
- [function `deflateFixed`](#function-deflatefixed)
  - [`deflateFixed()`](#deflateFixed)
- [function `gzipCompress`](#function-gzipcompress)
  - [`gzipCompress()`](#gzipCompress)
- [function `gzipDecompress`](#function-gzipdecompress)
  - [`gzipDecompress()`](#gzipDecompress)

## Imports

- `uranite.encoding.binary`
  - `copyBytes`
- `uranite.encoding.crc32`
  - `crc32`
- `uranite.io.syscall`
  - `readByteAt`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `GZIP_MAGIC_1`

## const `GZIP_MAGIC_2`

## const `GZIP_METHOD_DEFLATE`

## const `GZIP_HEADER_SIZE`

## const `GZIP_TRAILER_SIZE`

## const `DEFLATE_MAX_BITS`

## const `DEFLATE_MAX_LIT_CODES`

## const `DEFLATE_MAX_DIST_CODES`

## const `DEFLATE_MAX_CODES`

## const `DEFLATE_CODE_LENGTH_ORDER_COUNT`

## const `DEFLATE_WINDOW_SIZE`

## const `DEFLATE_HASH_BITS`

## const `DEFLATE_HASH_SIZE`

## const `DEFLATE_MIN_MATCH`

## const `DEFLATE_MAX_MATCH`

## const `CODE_LENGTH_ORDER_ADDRESS`

## class `BitReader`

Reads individual bits from a byte stream, LSB first within each byte. Maintains a bit buffer for efficient multi-bit reads.

### Fields

| Name | Type | Access |
|------|------|--------|
| `sourceAddress` | `I64` | public |
| `sourceLength` | `I64` | public |
| `bytePosition` | `I64` | public |
| `bitBuffer` | `I64` | public |
| `bitsAvailable` | `I64` | public |

### Methods

#### `function BitReader( self, I64 sourceAddress, I64 sourceLength ) -> Void`

**Parameters**:

- `sourceAddress` (`I64`)
- `Address of compressed data.`
- `sourceLength` (`I64`)
- `Length of compressed data.`

#### `function readBits( self, I64 bitCount ) -> I64`

Read up to 25 bits from the stream.

**Parameters**:

- `bitCount` (`I64`)
- `Number of bits to read` (`1-25`)

**Returns**: — I64:
Value read, or -1 on EOF.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function alignToByte( self ) -> Void`

Discard remaining bits in the current byte.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `BitWriter`

Writes individual bits to a byte stream, LSB first within each byte.

### Fields

| Name | Type | Access |
|------|------|--------|
| `destAddress` | `I64` | public |
| `destCapacity` | `I64` | public |
| `bytePosition` | `I64` | public |
| `bitBuffer` | `I64` | public |
| `bitsUsed` | `I64` | public |

### Methods

#### `function BitWriter( self, I64 destAddress, I64 destCapacity ) -> Void`

**Parameters**:

- `destAddress` (`I64`)
- `Output buffer address.`
- `destCapacity` (`I64`)
- `Output buffer capacity.`

#### `function writeBits( self, I64 value, I64 bitCount ) -> Void`

Write up to 25 bits to the stream.

**Parameters**:

- `value` (`I64`)
- `Value to write.`
- `bitCount` (`I64`)
- `Number of bits` (`1-25`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function flush( self ) -> Void`

Flush remaining bits as a final byte.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `HuffmanTable`

Huffman code table for DEFLATE decoding. Stores code counts per bit length and symbol values for fast decoding.

### Fields

| Name | Type | Access |
|------|------|--------|
| `countsAddress` | `I64` | public |
| `symbolsAddress` | `I64` | public |
| `maxSymbols` | `I64` | public |

### Methods

#### `function HuffmanTable( self, I64 maxSymbols ) -> Void`

**Parameters**:

- `maxSymbols` (`I64`)
- `Maximum number of symbols.`

#### `function build( self, I64 lengthsAddress, I64 symbolCount ) -> Boolean`

Build the Huffman table from an array of code lengths.

**Parameters**:

- `lengthsAddress` (`I64`)
- `Address of I32 code lengths per symbol.`
- `symbolCount` (`I64`)
- `Number of symbols.`

**Returns**: — Boolean:
True if table was built successfully.

**Complexity**:
- Time: `O(s) where s is symbolCount`
- Space: `O(1)`

#### `function decode( self, BitReader reader ) -> I64`

Decode one symbol from the bit stream using this table.

**Parameters**:

- `reader` (`BitReader`)
- `Bit reader positioned at the code.`

**Returns**: — I64:
Decoded symbol value, or -1 on error.

**Complexity**:
- Time: `O(code length), max DEFLATE_MAX_BITS`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release table storage.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `buildFixedLitLenTable`

Build the fixed literal/length Huffman table per RFC 1951 3.2.6.

**Parameters**:

- `table` (`HuffmanTable`)
- `Table to populate.`

**Complexity**:
- Time: `O(288)`
- Space: `O(288)`

### Methods

#### `function buildFixedLitLenTable( HuffmanTable table ) -> Void`

Build the fixed literal/length Huffman table per RFC 1951 3.2.6.

**Parameters**:

- `table` (`HuffmanTable`)
- `Table to populate.`

**Complexity**:
- Time: `O(288)`
- Space: `O(288)`

## function `buildFixedDistTable`

Build the fixed distance Huffman table per RFC 1951 3.2.6.

**Parameters**:

- `table` (`HuffmanTable`)
- `Table to populate.`

**Complexity**:
- Time: `O(32)`
- Space: `O(32)`

### Methods

#### `function buildFixedDistTable( HuffmanTable table ) -> Void`

Build the fixed distance Huffman table per RFC 1951 3.2.6.

**Parameters**:

- `table` (`HuffmanTable`)
- `Table to populate.`

**Complexity**:
- Time: `O(32)`
- Space: `O(32)`

## function `getLengthBase`

Get base length for length symbol (257-285).

**Parameters**:

- `symbol` (`I64`)
- `Length symbol code.`

**Returns**: — I64:
Base length value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getLengthBase( I64 symbol ) -> I64`

Get base length for length symbol (257-285).

**Parameters**:

- `symbol` (`I64`)
- `Length symbol code.`

**Returns**: — I64:
Base length value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `getLengthExtraBits`

Get number of extra bits for length symbol.

**Parameters**:

- `symbol` (`I64`)
- `Length symbol code` (`257-285`)

**Returns**: — I64:
Number of extra bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getLengthExtraBits( I64 symbol ) -> I64`

Get number of extra bits for length symbol.

**Parameters**:

- `symbol` (`I64`)
- `Length symbol code` (`257-285`)

**Returns**: — I64:
Number of extra bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `getDistBase`

Get base distance for distance symbol (0-29).

**Parameters**:

- `symbol` (`I64`)
- `Distance symbol code.`

**Returns**: — I64:
Base distance value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getDistBase( I64 symbol ) -> I64`

Get base distance for distance symbol (0-29).

**Parameters**:

- `symbol` (`I64`)
- `Distance symbol code.`

**Returns**: — I64:
Base distance value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `getDistExtraBits`

Get number of extra bits for distance symbol.

**Parameters**:

- `symbol` (`I64`)
- `Distance symbol code` (`0-29`)

**Returns**: — I64:
Number of extra bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getDistExtraBits( I64 symbol ) -> I64`

Get number of extra bits for distance symbol.

**Parameters**:

- `symbol` (`I64`)
- `Distance symbol code` (`0-29`)

**Returns**: — I64:
Number of extra bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `inflate`

Decompress DEFLATE-compressed data (RFC 1951).

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed data.`
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
- Space: `O(Huffman tables)`

### Methods

#### `function inflate( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Decompress DEFLATE-compressed data (RFC 1951).

**Parameters**:

- `inputAddress` (`I64`)
- `Address of compressed data.`
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
- Space: `O(Huffman tables)`

## function `deflateFixed`

Compress data using DEFLATE with fixed Huffman codes and LZ77 matching. Emits a single final block.

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
Compressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(HASH_TABLE_SIZE) for hash table`

### Methods

#### `function deflateFixed( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Compress data using DEFLATE with fixed Huffman codes and LZ77 matching. Emits a single final block.

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
Compressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(HASH_TABLE_SIZE) for hash table`

## function `gzipCompress`

Compress data with gzip framing (RFC 1952).

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
Total gzip output size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(n) for deflate buffer`

### Methods

#### `function gzipCompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Compress data with gzip framing (RFC 1952).

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
Total gzip output size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is inputLength`
- Space: `O(n) for deflate buffer`

## function `gzipDecompress`

Decompress gzip-formatted data (RFC 1952).

**Parameters**:

- `inputAddress` (`I64`)
- `Compressed gzip data address.`
- `inputLength` (`I64`)
- `Length of compressed data.`
- `outputAddress` (`I64`)
- `Output buffer address.`
- `outputCapacity` (`I64`)
- `Output buffer capacity.`

**Returns**: — I64:
Decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is decompressed size`
- Space: `O(Huffman tables)`

### Methods

#### `function gzipDecompress( I64 inputAddress, I64 inputLength, I64 outputAddress, I64 outputCapacity ) -> I64`

Decompress gzip-formatted data (RFC 1952).

**Parameters**:

- `inputAddress` (`I64`)
- `Compressed gzip data address.`
- `inputLength` (`I64`)
- `Length of compressed data.`
- `outputAddress` (`I64`)
- `Output buffer address.`
- `outputCapacity` (`I64`)
- `Output buffer capacity.`

**Returns**: — I64:
Decompressed size, or -1 on failure.

**Complexity**:
- Time: `O(n) where n is decompressed size`
- Space: `O(Huffman tables)`

