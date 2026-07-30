# uranite.encoding.crc32

## Table of Contents

- [Imports](#imports)
- [const `CRC32_POLYNOMIAL`](#const-crc32-polynomial)
- [const `CRC32_INITIAL`](#const-crc32-initial)
- [const `CRC32_TABLE_ENTRIES`](#const-crc32-table-entries)
- [const `CRC32_TABLE_BYTE_SIZE`](#const-crc32-table-byte-size)
- [const `tableAddress`](#const-tableaddress)
- [const `tableInitialized`](#const-tableinitialized)
- [function `initializeCrc32Table`](#function-initializecrc32table)
  - [`initializeCrc32Table()`](#initializeCrc32Table)
- [function `crc32Update`](#function-crc32update)
  - [`crc32Update()`](#crc32Update)
- [function `crc32Finalize`](#function-crc32finalize)
  - [`crc32Finalize()`](#crc32Finalize)
- [function `crc32`](#function-crc32)
  - [`crc32()`](#crc32)
- [function `crc32String`](#function-crc32string)
  - [`crc32String()`](#crc32String)

## Imports

- `uranite.io.syscall`
  - `readByteAt`
  - `readI64At`
  - `stringLen`
  - `stringToPtr`
  - `writeI64At`
- `uranite.memory.allocator`
  - `alloc`

## const `CRC32_POLYNOMIAL`

Standard CRC-32 polynomial 0xEDB88320 (bit-reversed form).

## const `CRC32_INITIAL`

Initial CRC value (all bits set, 0xFFFFFFFF).

## const `CRC32_TABLE_ENTRIES`

Number of entries in the CRC lookup table.

## const `CRC32_TABLE_BYTE_SIZE`

Byte size of the lookup table (256 entries * 8 bytes each).

## const `tableAddress`

Lazily initialized address of the 256-entry CRC-32 lookup table.

## const `tableInitialized`

Flag indicating whether the lookup table has been computed.

## function `initializeCrc32Table`

Compute the 256-entry standard CRC-32 lookup table from the ISO 3309 polynomial. Each entry represents the CRC contribution of a single byte value (0-255). Allocated once and reused for all subsequent CRC computations.

**Complexity**:
- Time: `O(1) (256 * 8 = 2048 iterations, constant)`
- Space: `O(1) (2048 bytes allocated)`

### Methods

#### `function initializeCrc32Table(  ) -> Void`

Compute the 256-entry standard CRC-32 lookup table from the ISO 3309 polynomial. Each entry represents the CRC contribution of a single byte value (0-255). Allocated once and reused for all subsequent CRC computations.

**Complexity**:
- Time: `O(1) (256 * 8 = 2048 iterations, constant)`
- Space: `O(1) (2048 bytes allocated)`

## function `crc32Update`

Update a running CRC-32 value by processing length bytes from the buffer. The lookup table is lazily initialized on first call.

**Parameters**:

- `currentCrc` (`I64`)
- `The current CRC value (use CRC32_INITIAL for a fresh`
- `computation, or a previous crc32Update result for`
- `incremental updates).`
- `bufferAddress` (`I64`)
- `Base address of the data buffer.`
- `length` (`I64`)
- `Number of bytes to process.`

**Returns**: — I64:
The updated CRC-32 value (not yet finalized).

**Complexity**:
- Time: `O(n) where n is length`
- Space: `O(1)`

### Methods

#### `function crc32Update( I64 currentCrc, I64 bufferAddress, I64 length ) -> I64`

Update a running CRC-32 value by processing length bytes from the buffer. The lookup table is lazily initialized on first call.

**Parameters**:

- `currentCrc` (`I64`)
- `The current CRC value (use CRC32_INITIAL for a fresh`
- `computation, or a previous crc32Update result for`
- `incremental updates).`
- `bufferAddress` (`I64`)
- `Base address of the data buffer.`
- `length` (`I64`)
- `Number of bytes to process.`

**Returns**: — I64:
The updated CRC-32 value (not yet finalized).

**Complexity**:
- Time: `O(n) where n is length`
- Space: `O(1)`

## function `crc32Finalize`

Finalize a CRC-32 value by XOR-ing with 0xFFFFFFFF.

**Parameters**:

- `crc` (`I64`)
- `The running CRC value from crc32Update.`

**Returns**: — I64:
The finalized CRC-32 checksum.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function crc32Finalize( I64 crc ) -> I64`

Finalize a CRC-32 value by XOR-ing with 0xFFFFFFFF.

**Parameters**:

- `crc` (`I64`)
- `The running CRC value from crc32Update.`

**Returns**: — I64:
The finalized CRC-32 checksum.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `crc32`

Compute the standard CRC-32 checksum of length bytes starting at bufferAddress. Convenience function combining initialization, update, and finalization.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the data buffer.`
- `length` (`I64`)
- `Number of bytes to checksum.`

**Returns**: — I64:
The CRC-32 checksum of the data.

**Complexity**:
- Time: `O(n) where n is length`
- Space: `O(1)`

### Methods

#### `function crc32( I64 bufferAddress, I64 length ) -> I64`

Compute the standard CRC-32 checksum of length bytes starting at bufferAddress. Convenience function combining initialization, update, and finalization.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the data buffer.`
- `length` (`I64`)
- `Number of bytes to checksum.`

**Returns**: — I64:
The CRC-32 checksum of the data.

**Complexity**:
- Time: `O(n) where n is length`
- Space: `O(1)`

## function `crc32String`

Compute the standard CRC-32 checksum of a string.

**Parameters**:

- `data` (`String`)
- `The input data to checksum.`

**Returns**: — I64:
The CRC-32 checksum.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

### Methods

#### `function crc32String( String data ) -> I64`

Compute the standard CRC-32 checksum of a string.

**Parameters**:

- `data` (`String`)
- `The input data to checksum.`

**Returns**: — I64:
The CRC-32 checksum.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

