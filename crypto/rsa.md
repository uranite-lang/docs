# uranite.crypto.rsa

## Table of Contents

- [Imports](#imports)
- [const `RSA_BLOCK_TYPE_ENCRYPT`](#const-rsa-block-type-encrypt)
- [const `RSA_PKCS1_PADDING_MIN`](#const-rsa-pkcs1-padding-min)
- [class `RsaPublicKey`](#class-rsapublickey)
  - [`RsaPublicKey()`](#RsaPublicKey)
  - [`destroy()`](#destroy)
- [function `rsaPublicKeyFromDer`](#function-rsapublickeyfromder)
  - [`rsaPublicKeyFromDer()`](#rsaPublicKeyFromDer)
- [function `rsaRawEncrypt`](#function-rsarawencrypt)
  - [`rsaRawEncrypt()`](#rsaRawEncrypt)
- [function `rsaEncryptPkcs1v15`](#function-rsaencryptpkcs1v15)
  - [`rsaEncryptPkcs1v15()`](#rsaEncryptPkcs1v15)
- [function `rsaPublicKeyFromPem`](#function-rsapublickeyfrompem)
  - [`rsaPublicKeyFromPem()`](#rsaPublicKeyFromPem)
- [function `rsaEncryptString`](#function-rsaencryptstring)
  - [`rsaEncryptString()`](#rsaEncryptString)

## Imports

- `uranite.crypto.bigint`
  - `BigInt`
  - `bigintFromBytes`
  - `bigintModExp`
  - `bigintToBytes`
- `uranite.crypto.random`
  - `randomBytes`
- `uranite.encoding.binary`
  - `copyBytes`
- `uranite.encoding.der`
  - `DER_TAG_BIT_STRING`
  - `DER_TAG_INTEGER`
  - `DER_TAG_SEQUENCE`
  - `DerElement`
  - `derGetIntegerBytes`
  - `derIterateSequence`
  - `derParseBitString`
  - `derParseElement`
  - `derParseSequence`
- `uranite.encoding.pem`
  - `PemBlock`
  - `pemDecodeString`
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

## const `RSA_BLOCK_TYPE_ENCRYPT`

## const `RSA_PKCS1_PADDING_MIN`

## class `RsaPublicKey`

RSA public key containing the modulus (n) and public exponent (e).

### Fields

| Name | Type | Access |
|------|------|--------|
| `modulus` | `BigInt` | public |
| `exponent` | `BigInt` | public |
| `modulusByteLength` | `I64` | public |

### Methods

#### `function RsaPublicKey( self, BigInt modulus, BigInt exponent, I64 modulusByteLength ) -> Void`

**Parameters**:

- `modulus` (`BigInt`)
- `RSA modulus n.`
- `exponent` (`BigInt`)
- `Public exponent e.`
- `modulusByteLength` (`I64`)
- `Byte length of the modulus.`

#### `function destroy( self ) -> Void`

Release key storage.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `rsaPublicKeyFromDer`

Parse an RSA public key from DER-encoded SubjectPublicKeyInfo.

The structure is: SEQUENCE { SEQUENCE { OID, NULL }  -- algorithm identifier BIT STRING {            -- subjectPublicKey SEQUENCE { INTEGER n       -- modulus INTEGER e       -- exponent } } }

**Parameters**:

- `derAddress` (`I64`)
- `Address of DER data.`
- `derLength` (`I64`)
- `Length of DER data.`

**Returns**: — RsaPublicKey:
Parsed public key.

**Complexity**:
- Time: `O(n) where n is key size in bytes`
- Space: `O(key size in limbs)`

### Methods

#### `function rsaPublicKeyFromDer( I64 derAddress, I64 derLength ) -> RsaPublicKey`

Parse an RSA public key from DER-encoded SubjectPublicKeyInfo.

The structure is: SEQUENCE { SEQUENCE { OID, NULL }  -- algorithm identifier BIT STRING {            -- subjectPublicKey SEQUENCE { INTEGER n       -- modulus INTEGER e       -- exponent } } }

**Parameters**:

- `derAddress` (`I64`)
- `Address of DER data.`
- `derLength` (`I64`)
- `Length of DER data.`

**Returns**: — RsaPublicKey:
Parsed public key.

**Complexity**:
- Time: `O(n) where n is key size in bytes`
- Space: `O(key size in limbs)`

## function `rsaRawEncrypt`

Raw RSA encryption: ciphertext = message^e mod n.

**Parameters**:

- `message` (`BigInt`)
- `Plaintext message as BigInt` (`must be < n`)
- `publicKey` (`RsaPublicKey`)
- `RSA public key.`

**Returns**: — BigInt:
Encrypted ciphertext as BigInt.

**Complexity**:
- Time: `O(k^2 * e) where k is key size in limbs, e is exponent bits`
- Space: `O(k) for temporaries`

### Methods

#### `function rsaRawEncrypt( BigInt message, RsaPublicKey publicKey ) -> BigInt`

Raw RSA encryption: ciphertext = message^e mod n.

**Parameters**:

- `message` (`BigInt`)
- `Plaintext message as BigInt` (`must be < n`)
- `publicKey` (`RsaPublicKey`)
- `RSA public key.`

**Returns**: — BigInt:
Encrypted ciphertext as BigInt.

**Complexity**:
- Time: `O(k^2 * e) where k is key size in limbs, e is exponent bits`
- Space: `O(k) for temporaries`

## function `rsaEncryptPkcs1v15`

Encrypt plaintext using PKCS#1 v1.5 type 2 padding (for key exchange). Constructs the padded block: 0x00 || 0x02 || PS (non-zero random) || 0x00 || M

**Parameters**:

- `plaintextAddress` (`I64`)
- `Address of plaintext` (`e.g. 48-byte premaster secret`)
- `plaintextLength` (`I64`)
- `Length of plaintext.`
- `publicKey` (`RsaPublicKey`)
- `Server's RSA public key.`
- `outputAddress` (`I64`)
- `Address of output buffer for ciphertext.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Ciphertext length (= modulus byte length), or -1 on error.

**Complexity**:
- Time: `O(k^2 * e) dominated by modexp`
- Space: `O(k) for padded block and BigInt temporaries`

### Methods

#### `function rsaEncryptPkcs1v15( I64 plaintextAddress, I64 plaintextLength, RsaPublicKey publicKey, I64 outputAddress, I64 outputCapacity ) -> I64`

Encrypt plaintext using PKCS#1 v1.5 type 2 padding (for key exchange). Constructs the padded block: 0x00 || 0x02 || PS (non-zero random) || 0x00 || M

**Parameters**:

- `plaintextAddress` (`I64`)
- `Address of plaintext` (`e.g. 48-byte premaster secret`)
- `plaintextLength` (`I64`)
- `Length of plaintext.`
- `publicKey` (`RsaPublicKey`)
- `Server's RSA public key.`
- `outputAddress` (`I64`)
- `Address of output buffer for ciphertext.`
- `outputCapacity` (`I64`)
- `Capacity of output buffer.`

**Returns**: — I64:
Ciphertext length (= modulus byte length), or -1 on error.

**Complexity**:
- Time: `O(k^2 * e) dominated by modexp`
- Space: `O(k) for padded block and BigInt temporaries`

## function `rsaPublicKeyFromPem`

Parse an RSA public key from a PEM-encoded string. Decodes the PEM envelope and delegates to rsaPublicKeyFromDer for SubjectPublicKeyInfo parsing.

**Parameters**:

- `pemData` (`String`)
- `PEM-encoded public key string` (`including BEGIN/END markers`)

**Returns**: — RsaPublicKey:
Parsed public key with modulus and exponent.

**Complexity**:
- Time: `O(n) where n is PEM data length`
- Space: `O(key size in limbs)`

### Methods

#### `function rsaPublicKeyFromPem( String pemData ) -> RsaPublicKey`

Parse an RSA public key from a PEM-encoded string. Decodes the PEM envelope and delegates to rsaPublicKeyFromDer for SubjectPublicKeyInfo parsing.

**Parameters**:

- `pemData` (`String`)
- `PEM-encoded public key string` (`including BEGIN/END markers`)

**Returns**: — RsaPublicKey:
Parsed public key with modulus and exponent.

**Complexity**:
- Time: `O(n) where n is PEM data length`
- Space: `O(key size in limbs)`

## function `rsaEncryptString`

Encrypt a plaintext string using PKCS#1 v1.5 padding and return the ciphertext as a raw byte string.

**Parameters**:

- `plaintext` (`String`)
- `The plaintext data to encrypt.`
- `publicKey` (`RsaPublicKey`)
- `The RSA public key to encrypt with.`

**Returns**: — String:
The ciphertext bytes as a string.

**Complexity**:
- Time: `O(k^2 * e) dominated by modular exponentiation`
- Space: `O(k) where k is key size in bytes`

### Methods

#### `function rsaEncryptString( String plaintext, RsaPublicKey publicKey ) -> String`

Encrypt a plaintext string using PKCS#1 v1.5 padding and return the ciphertext as a raw byte string.

**Parameters**:

- `plaintext` (`String`)
- `The plaintext data to encrypt.`
- `publicKey` (`RsaPublicKey`)
- `The RSA public key to encrypt with.`

**Returns**: — String:
The ciphertext bytes as a string.

**Complexity**:
- Time: `O(k^2 * e) dominated by modular exponentiation`
- Space: `O(k) where k is key size in bytes`

