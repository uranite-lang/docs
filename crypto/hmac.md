# uranite.crypto.hmac

## Table of Contents

- [Imports](#imports)
- [const `IPAD_BYTE`](#const-ipad-byte)
- [const `OPAD_BYTE`](#const-opad-byte)
- [function `hmacSha256`](#function-hmacsha256)
  - [`hmacSha256()`](#hmacSha256)
- [function `hmacSha512`](#function-hmacsha512)
  - [`hmacSha512()`](#hmacSha512)
- [function `bytesToHex`](#function-bytestohex)
- [function `hmacSha256Digest`](#function-hmacsha256digest)
  - [`hmacSha256Digest()`](#hmacSha256Digest)
- [function `hmacSha512Digest`](#function-hmacsha512digest)
  - [`hmacSha512Digest()`](#hmacSha512Digest)

## Imports

- `uranite.crypto.sha256`
  - `SHA256_BLOCK_SIZE`
  - `SHA256_DIGEST_SIZE`
  - `Sha256`
- `uranite.crypto.sha512`
  - `SHA512_BLOCK_SIZE`
  - `SHA512_DIGEST_SIZE`
  - `Sha512`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `IPAD_BYTE`

Inner padding byte 0x36.

## const `OPAD_BYTE`

Outer padding byte 0x5C.

## function `hmacSha256`

Compute HMAC-SHA-256 per RFC 2104. If the key is longer than the block size (64 bytes), it is first hashed with SHA-256 to produce a 32-byte derived key.

**Parameters**:

- `keyAddress` (`I64`)
- `Base address of the HMAC key.`
- `keyLength` (`I64`)
- `Length of the key in bytes.`
- `dataAddress` (`I64`)
- `Base address of the message data.`
- `dataLength` (`I64`)
- `Length of the message in bytes.`
- `outputAddress` (`I64`)
- `Address to write the 32-byte HMAC result.`

**Complexity**:
- Time: `O(n) where n is keyLength + dataLength`
- Space: `O(1) (fixed overhead for padding buffers)`

### Methods

#### `function hmacSha256( I64 keyAddress, I64 keyLength, I64 dataAddress, I64 dataLength, I64 outputAddress ) -> Void`

Compute HMAC-SHA-256 per RFC 2104. If the key is longer than the block size (64 bytes), it is first hashed with SHA-256 to produce a 32-byte derived key.

**Parameters**:

- `keyAddress` (`I64`)
- `Base address of the HMAC key.`
- `keyLength` (`I64`)
- `Length of the key in bytes.`
- `dataAddress` (`I64`)
- `Base address of the message data.`
- `dataLength` (`I64`)
- `Length of the message in bytes.`
- `outputAddress` (`I64`)
- `Address to write the 32-byte HMAC result.`

**Complexity**:
- Time: `O(n) where n is keyLength + dataLength`
- Space: `O(1) (fixed overhead for padding buffers)`

## function `hmacSha512`

Compute HMAC-SHA-512 per RFC 2104. If the key is longer than the block size (128 bytes), it is first hashed with SHA-512 to produce a 64-byte derived key.

**Parameters**:

- `keyAddress` (`I64`)
- `Base address of the HMAC key.`
- `keyLength` (`I64`)
- `Length of the key in bytes.`
- `dataAddress` (`I64`)
- `Base address of the message data.`
- `dataLength` (`I64`)
- `Length of the message in bytes.`
- `outputAddress` (`I64`)
- `Address to write the 64-byte HMAC result.`

**Complexity**:
- Time: `O(n) where n is keyLength + dataLength`
- Space: `O(1) (fixed overhead for padding buffers)`

### Methods

#### `function hmacSha512( I64 keyAddress, I64 keyLength, I64 dataAddress, I64 dataLength, I64 outputAddress ) -> Void`

Compute HMAC-SHA-512 per RFC 2104. If the key is longer than the block size (128 bytes), it is first hashed with SHA-512 to produce a 64-byte derived key.

**Parameters**:

- `keyAddress` (`I64`)
- `Base address of the HMAC key.`
- `keyLength` (`I64`)
- `Length of the key in bytes.`
- `dataAddress` (`I64`)
- `Base address of the message data.`
- `dataLength` (`I64`)
- `Length of the message in bytes.`
- `outputAddress` (`I64`)
- `Address to write the 64-byte HMAC result.`

**Complexity**:
- Time: `O(n) where n is keyLength + dataLength`
- Space: `O(1) (fixed overhead for padding buffers)`

## function `bytesToHex`

Convert raw bytes to a lowercase hexadecimal string.

**Parameters**:

- `dataAddress` (`I64`)
- `Address of the byte data.`
- `dataLength` (`I64`)
- `Number of bytes.`

**Returns**: — String:
Hex-encoded string (2 characters per byte).

**Complexity**:
- Time: `O(n) where n is dataLength`
- Space: `O(2n)`

### Methods

#### `function bytesToHex( I64 dataAddress, I64 dataLength ) -> String`

Convert raw bytes to a lowercase hexadecimal string.

**Parameters**:

- `dataAddress` (`I64`)
- `Address of the byte data.`
- `dataLength` (`I64`)
- `Number of bytes.`

**Returns**: — String:
Hex-encoded string (2 characters per byte).

**Complexity**:
- Time: `O(n) where n is dataLength`
- Space: `O(2n)`

## function `hmacSha256Digest`

Compute HMAC-SHA-256 over string inputs and return the result as a 64-character lowercase hexadecimal string.

**Parameters**:

- `key` (`String`)
- `The HMAC key.`
- `data` (`String`)
- `The message to authenticate.`

**Returns**: — String:
The 32-byte MAC as 64 hex characters.

**Complexity**:
- Time: `O(n) where n is key + data length`
- Space: `O(1)`

### Methods

#### `function hmacSha256Digest( String key, String data ) -> String`

Compute HMAC-SHA-256 over string inputs and return the result as a 64-character lowercase hexadecimal string.

**Parameters**:

- `key` (`String`)
- `The HMAC key.`
- `data` (`String`)
- `The message to authenticate.`

**Returns**: — String:
The 32-byte MAC as 64 hex characters.

**Complexity**:
- Time: `O(n) where n is key + data length`
- Space: `O(1)`

## function `hmacSha512Digest`

Compute HMAC-SHA-512 over string inputs and return the result as a 128-character lowercase hexadecimal string.

**Parameters**:

- `key` (`String`)
- `The HMAC key.`
- `data` (`String`)
- `The message to authenticate.`

**Returns**: — String:
The 64-byte MAC as 128 hex characters.

**Complexity**:
- Time: `O(n) where n is key + data length`
- Space: `O(1)`

### Methods

#### `function hmacSha512Digest( String key, String data ) -> String`

Compute HMAC-SHA-512 over string inputs and return the result as a 128-character lowercase hexadecimal string.

**Parameters**:

- `key` (`String`)
- `The HMAC key.`
- `data` (`String`)
- `The message to authenticate.`

**Returns**: — String:
The 64-byte MAC as 128 hex characters.

**Complexity**:
- Time: `O(n) where n is key + data length`
- Space: `O(1)`

