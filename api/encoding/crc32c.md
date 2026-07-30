# uranite.encoding.crc32c

## Table of Contents

- [Imports](#imports)
- [const `CRC32C_POLYNOMIAL`](#const-crc32c-polynomial)
- [const `CRC32C_INITIAL`](#const-crc32c-initial)
- [const `CRC32C_TABLE_ENTRIES`](#const-crc32c-table-entries)
- [const `CRC32C_TABLE_BYTE_SIZE`](#const-crc32c-table-byte-size)
- [const `tableAddress`](#const-tableaddress)
- [const `tableInitialized`](#const-tableinitialized)
- [function `initializeCrc32cTable`](#function-initializecrc32ctable)
  - [`initializeCrc32cTable()`](#initializeCrc32cTable)
- [function `crc32cUpdate`](#function-crc32cupdate)
  - [`crc32cUpdate()`](#crc32cUpdate)
- [function `crc32cFinalize`](#function-crc32cfinalize)
  - [`crc32cFinalize()`](#crc32cFinalize)
- [function `crc32c`](#function-crc32c)
  - [`crc32c()`](#crc32c)
- [function `crc32cString`](#function-crc32cstring)
  - [`crc32cString()`](#crc32cString)

## Imports

- `uranite.io.syscall`
  - `readByteAt`
  - `readI64At`
  - `stringLen`
  - `stringToPtr`
  - `writeI64At`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `CRC32C_POLYNOMIAL`

Castagnoli polynomial 0x82F63B78 for CRC-32C.

## const `CRC32C_INITIAL`

Initial CRC value (all bits set, 0xFFFFFFFF).

## const `CRC32C_TABLE_ENTRIES`

Number of entries in the CRC lookup table.

## const `CRC32C_TABLE_BYTE_SIZE`

Byte size of the lookup table (256 entries * 8 bytes each).

## const `tableAddress`

Lazily initialized address of the 256-entry CRC-32C lookup table.

## const `tableInitialized`

Flag indicating whether the lookup table has been computed.

## function `initializeCrc32cTable`

Compute the 256-entry CRC-32C lookup table from the Castagnoli polynomial. Each entry represents the CRC contribution of a single byte value (0-255). The table is allocated once and reused for all subsequent CRC computations.

**Complexity**:
- Time: `O(1) (256 * 8 = 2048 iterations, constant)`
- Space: `O(1) (2048 bytes allocated)`

### Methods

#### `function initializeCrc32cTable(  ) -> Void`

Compute the 256-entry CRC-32C lookup table from the Castagnoli polynomial. Each entry represents the CRC contribution of a single byte value (0-255). The table is allocated once and reused for all subsequent CRC computations.

**Complexity**:
- Time: `O(1) (256 * 8 = 2048 iterations, constant)`
- Space: `O(1) (2048 bytes allocated)`

## function `crc32cUpdate`

Update a running CRC-32C value by processing length bytes from the buffer. The lookup table is lazily initialized on first call.

**Parameters**:

- `currentCrc` (`I64`)
- `The current CRC value (use CRC32C_INITIAL for a fresh`
- `computation, or a previous crc32cUpdate result for`
- `incremental updates).`
- `bufferAddress` (`I64`)
- `Base address of the data buffer.`
- `length` (`I64`)
- `Number of bytes to process.`

**Returns**: — I64:
The updated CRC-32C value (not yet finalized).

**Complexity**:
- Time: `O(n) where n is length`
- Space: `O(1)`

### Methods

#### `function crc32cUpdate( I64 currentCrc, I64 bufferAddress, I64 length ) -> I64`

Update a running CRC-32C value by processing length bytes from the buffer. The lookup table is lazily initialized on first call.

**Parameters**:

- `currentCrc` (`I64`)
- `The current CRC value (use CRC32C_INITIAL for a fresh`
- `computation, or a previous crc32cUpdate result for`
- `incremental updates).`
- `bufferAddress` (`I64`)
- `Base address of the data buffer.`
- `length` (`I64`)
- `Number of bytes to process.`

**Returns**: — I64:
The updated CRC-32C value (not yet finalized).

**Complexity**:
- Time: `O(n) where n is length`
- Space: `O(1)`

## function `crc32cFinalize`

Finalize a CRC-32C value by XOR-ing with 0xFFFFFFFF. Call this after all data has been processed via crc32cUpdate.

**Parameters**:

- `crc` (`I64`)
- `The running CRC value from crc32cUpdate.`

**Returns**: — I64:
The finalized CRC-32C checksum.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function crc32cFinalize( I64 crc ) -> I64`

Finalize a CRC-32C value by XOR-ing with 0xFFFFFFFF. Call this after all data has been processed via crc32cUpdate.

**Parameters**:

- `crc` (`I64`)
- `The running CRC value from crc32cUpdate.`

**Returns**: — I64:
The finalized CRC-32C checksum.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `crc32c`

Compute the CRC-32C checksum of length bytes starting at bufferAddress. Convenience function combining initialization, update, and finalization.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the data buffer.`
- `length` (`I64`)
- `Number of bytes to checksum.`

**Returns**: — I64:
The CRC-32C checksum of the data.

**Complexity**:
- Time: `O(n) where n is length`
- Space: `O(1)`

### Methods

#### `function crc32c( I64 bufferAddress, I64 length ) -> I64`

Compute the CRC-32C checksum of length bytes starting at bufferAddress. Convenience function combining initialization, update, and finalization.

**Parameters**:

- `bufferAddress` (`I64`)
- `Base address of the data buffer.`
- `length` (`I64`)
- `Number of bytes to checksum.`

**Returns**: — I64:
The CRC-32C checksum of the data.

**Complexity**:
- Time: `O(n) where n is length`
- Space: `O(1)`

## function `crc32cString`

Compute the CRC-32C (Castagnoli) checksum of a string.

**Parameters**:

- `data` (`String`)
- `The input data to checksum.`

**Returns**: — I64:
The CRC-32C checksum.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

### Methods

#### `function crc32cString( String data ) -> I64`

Compute the CRC-32C (Castagnoli) checksum of a string.

**Parameters**:

- `data` (`String`)
- `The input data to checksum.`

**Returns**: — I64:
The CRC-32C checksum.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

