# uranite.crypto.cipher

## Table of Contents

- [Imports](#imports)
- [class `AesCipher`](#class-aescipher)
  - [`AesCipher()`](#AesCipher)
  - [`init()`](#init)
  - [`initFromBytes()`](#initFromBytes)
  - [`hasHardwareSupport()`](#hasHardwareSupport)
  - [`getRounds()`](#getRounds)
  - [`getKeySize()`](#getKeySize)
  - [`isInitialized()`](#isInitialized)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
- `uranite.os.crypto.aes`
  - `AES_128`
  - `AES_256`
  - `AES_BLOCK_SIZE`
  - `AesContext`

## class `AesCipher`

AES block cipher supporting 128-bit and 256-bit key sizes. Provides single-block encrypt and decrypt operations. Wraps the kernel AesContext with initialization validation.

### Fields

| Name | Type | Access |
|------|------|--------|
| `context` | `AesContext` | protect |
| `keyBits` | `I64` | protect |

### Methods

#### `function AesCipher( self, I64 keyBits ) -> Void`

Construct an AES cipher with the specified key size (AES_128 or AES_256).

**Parameters**:

- `keyBits` (`I64`)
- `Key size in bits. Use AES_128` (`128`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function init( self, I64 k0, I64 k1, I64 k2, I64 k3 ) -> Void`

Initialize the cipher with a key specified as four 64-bit words. For AES-128, only k0 and k1 are used. For AES-256, all four words form the full key.

#### `function initFromBytes( self, String keyBytes ) -> Void`

Initialize the cipher from a raw byte string. For AES-128, the string must be at least 16 bytes; for AES-256, at least 32 bytes. Bytes are packed into 64-bit words in little-endian order.

**Parameters**:

- `keyBytes` (`String`)
- `Raw key bytes as a string.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function hasHardwareSupport( self ) -> Boolean`

Return whether AES-NI hardware acceleration is available. 

#### `function getRounds( self ) -> I64`

Return the number of AES rounds (10 for 128, 14 for 256). 

#### `function getKeySize( self ) -> I64`

Return the configured key size in bits. 

#### `function isInitialized( self ) -> Boolean`

Return whether the cipher has been initialized with a key. 

#### `function destroy( self ) -> Void`

Securely destroy the cipher context and key material.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

