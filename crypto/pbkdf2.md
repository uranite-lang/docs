# uranite.crypto.pbkdf2

## Table of Contents

- [Imports](#imports)
- [function `pbkdf2HmacSha256`](#function-pbkdf2hmacsha256)
  - [`pbkdf2HmacSha256()`](#pbkdf2HmacSha256)
- [function `pbkdf2HmacSha512`](#function-pbkdf2hmacsha512)
  - [`pbkdf2HmacSha512()`](#pbkdf2HmacSha512)
- [function `bytesToHex`](#function-bytestohex)
- [function `pbkdf2Sha256Derive`](#function-pbkdf2sha256derive)
  - [`pbkdf2Sha256Derive()`](#pbkdf2Sha256Derive)
- [function `pbkdf2Sha512Derive`](#function-pbkdf2sha512derive)
  - [`pbkdf2Sha512Derive()`](#pbkdf2Sha512Derive)

## Imports

- `uranite.crypto.hmac`
  - `hmacSha256`
  - `hmacSha512`
- `uranite.crypto.sha256`
  - `SHA256_DIGEST_SIZE`
- `uranite.crypto.sha512`
  - `SHA512_DIGEST_SIZE`
- `uranite.encoding.binary`
  - `writeI32Be`
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

## function `pbkdf2HmacSha256`

Derive a key from a password using PBKDF2 with HMAC-SHA-256 as the pseudorandom function per RFC 2898 section 5.2.

For each block of derived key material, computes: U1 = HMAC(password, salt || INT(blockIndex)) U2 = HMAC(password, U1) ... Ui = HMAC(password, U_{i-1}) DK_block = U1 ^ U2 ^ ... ^ Ui

**Parameters**:

- `passwordAddress` (`I64`)
- `Base address of the password bytes.`
- `passwordLength` (`I64`)
- `Length of the password in bytes.`
- `saltAddress` (`I64`)
- `Base address of the salt bytes.`
- `saltLength` (`I64`)
- `Length of the salt in bytes.`
- `iterations` (`I64`)
- `Number of HMAC iterations` (`SCRAM typically uses 4096+`)
- `outputAddress` (`I64`)
- `Address to write the derived key.`
- `outputLength` (`I64`)
- `Desired length of the derived key in bytes.`

**Complexity**:
- Time: `O(iterations * ceil(outputLength / 32))`
- Space: `O(1) (fixed overhead for intermediate buffers)`

### Methods

#### `function pbkdf2HmacSha256( I64 passwordAddress, I64 passwordLength, I64 saltAddress, I64 saltLength, I64 iterations, I64 outputAddress, I64 outputLength ) -> Void`

Derive a key from a password using PBKDF2 with HMAC-SHA-256 as the pseudorandom function per RFC 2898 section 5.2.

For each block of derived key material, computes: U1 = HMAC(password, salt || INT(blockIndex)) U2 = HMAC(password, U1) ... Ui = HMAC(password, U_{i-1}) DK_block = U1 ^ U2 ^ ... ^ Ui

**Parameters**:

- `passwordAddress` (`I64`)
- `Base address of the password bytes.`
- `passwordLength` (`I64`)
- `Length of the password in bytes.`
- `saltAddress` (`I64`)
- `Base address of the salt bytes.`
- `saltLength` (`I64`)
- `Length of the salt in bytes.`
- `iterations` (`I64`)
- `Number of HMAC iterations` (`SCRAM typically uses 4096+`)
- `outputAddress` (`I64`)
- `Address to write the derived key.`
- `outputLength` (`I64`)
- `Desired length of the derived key in bytes.`

**Complexity**:
- Time: `O(iterations * ceil(outputLength / 32))`
- Space: `O(1) (fixed overhead for intermediate buffers)`

## function `pbkdf2HmacSha512`

Derive a key from a password using PBKDF2 with HMAC-SHA-512. Same algorithm as pbkdf2HmacSha256 but using SHA-512.

**Parameters**:

- `passwordAddress` (`I64`)
- `Base address of the password bytes.`
- `passwordLength` (`I64`)
- `Length of the password in bytes.`
- `saltAddress` (`I64`)
- `Base address of the salt bytes.`
- `saltLength` (`I64`)
- `Length of the salt in bytes.`
- `iterations` (`I64`)
- `Number of HMAC iterations.`
- `outputAddress` (`I64`)
- `Address to write the derived key.`
- `outputLength` (`I64`)
- `Desired length of the derived key in bytes.`

**Complexity**:
- Time: `O(iterations * ceil(outputLength / 64))`
- Space: `O(1)`

### Methods

#### `function pbkdf2HmacSha512( I64 passwordAddress, I64 passwordLength, I64 saltAddress, I64 saltLength, I64 iterations, I64 outputAddress, I64 outputLength ) -> Void`

Derive a key from a password using PBKDF2 with HMAC-SHA-512. Same algorithm as pbkdf2HmacSha256 but using SHA-512.

**Parameters**:

- `passwordAddress` (`I64`)
- `Base address of the password bytes.`
- `passwordLength` (`I64`)
- `Length of the password in bytes.`
- `saltAddress` (`I64`)
- `Base address of the salt bytes.`
- `saltLength` (`I64`)
- `Length of the salt in bytes.`
- `iterations` (`I64`)
- `Number of HMAC iterations.`
- `outputAddress` (`I64`)
- `Address to write the derived key.`
- `outputLength` (`I64`)
- `Desired length of the derived key in bytes.`

**Complexity**:
- Time: `O(iterations * ceil(outputLength / 64))`
- Space: `O(1)`

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

## function `pbkdf2Sha256Derive`

Derive a key from a password using PBKDF2-HMAC-SHA-256 and return the result as a lowercase hexadecimal string.

**Parameters**:

- `password` (`String`)
- `The password to derive from.`
- `salt` (`String`)
- `The salt value.`
- `iterations` (`I64`)
- `Number of HMAC iterations` (`typically 4096+`)
- `outputLength` (`I64`)
- `Desired derived key length in bytes.`

**Returns**: — String:
The derived key as a hex string (2 * outputLength characters).

**Complexity**:
- Time: `O(iterations * ceil(outputLength / 32))`
- Space: `O(outputLength)`

### Methods

#### `function pbkdf2Sha256Derive( String password, String salt, I64 iterations, I64 outputLength ) -> String`

Derive a key from a password using PBKDF2-HMAC-SHA-256 and return the result as a lowercase hexadecimal string.

**Parameters**:

- `password` (`String`)
- `The password to derive from.`
- `salt` (`String`)
- `The salt value.`
- `iterations` (`I64`)
- `Number of HMAC iterations` (`typically 4096+`)
- `outputLength` (`I64`)
- `Desired derived key length in bytes.`

**Returns**: — String:
The derived key as a hex string (2 * outputLength characters).

**Complexity**:
- Time: `O(iterations * ceil(outputLength / 32))`
- Space: `O(outputLength)`

## function `pbkdf2Sha512Derive`

Derive a key from a password using PBKDF2-HMAC-SHA-512 and return the result as a lowercase hexadecimal string.

**Parameters**:

- `password` (`String`)
- `The password to derive from.`
- `salt` (`String`)
- `The salt value.`
- `iterations` (`I64`)
- `Number of HMAC iterations.`
- `outputLength` (`I64`)
- `Desired derived key length in bytes.`

**Returns**: — String:
The derived key as a hex string (2 * outputLength characters).

**Complexity**:
- Time: `O(iterations * ceil(outputLength / 64))`
- Space: `O(outputLength)`

### Methods

#### `function pbkdf2Sha512Derive( String password, String salt, I64 iterations, I64 outputLength ) -> String`

Derive a key from a password using PBKDF2-HMAC-SHA-512 and return the result as a lowercase hexadecimal string.

**Parameters**:

- `password` (`String`)
- `The password to derive from.`
- `salt` (`String`)
- `The salt value.`
- `iterations` (`I64`)
- `Number of HMAC iterations.`
- `outputLength` (`I64`)
- `Desired derived key length in bytes.`

**Returns**: — String:
The derived key as a hex string (2 * outputLength characters).

**Complexity**:
- Time: `O(iterations * ceil(outputLength / 64))`
- Space: `O(outputLength)`

