# uranite.net.tls

## Table of Contents

- [Imports](#imports)
- [const `TLS_VERSION_10`](#const-tls-version-10)
- [const `TLS_VERSION_12`](#const-tls-version-12)
- [const `CONTENT_TYPE_CHANGE_CIPHER_SPEC`](#const-content-type-change-cipher-spec)
- [const `CONTENT_TYPE_ALERT`](#const-content-type-alert)
- [const `CONTENT_TYPE_HANDSHAKE`](#const-content-type-handshake)
- [const `CONTENT_TYPE_APPLICATION_DATA`](#const-content-type-application-data)
- [const `HANDSHAKE_CLIENT_HELLO`](#const-handshake-client-hello)
- [const `HANDSHAKE_SERVER_HELLO`](#const-handshake-server-hello)
- [const `HANDSHAKE_CERTIFICATE`](#const-handshake-certificate)
- [const `HANDSHAKE_SERVER_HELLO_DONE`](#const-handshake-server-hello-done)
- [const `HANDSHAKE_CLIENT_KEY_EXCHANGE`](#const-handshake-client-key-exchange)
- [const `HANDSHAKE_FINISHED`](#const-handshake-finished)
- [const `CIPHER_TLS_RSA_AES_128_GCM_SHA256`](#const-cipher-tls-rsa-aes-128-gcm-sha256)
- [const `AES_BLOCK_SIZE`](#const-aes-block-size)
- [const `AES_128_KEY_SIZE`](#const-aes-128-key-size)
- [const `AES_128_ROUNDS`](#const-aes-128-rounds)
- [const `GCM_IV_SIZE`](#const-gcm-iv-size)
- [const `GCM_TAG_SIZE`](#const-gcm-tag-size)
- [const `GCM_EXPLICIT_NONCE_SIZE`](#const-gcm-explicit-nonce-size)
- [const `TLS_RECORD_HEADER_SIZE`](#const-tls-record-header-size)
- [const `TLS_MAX_RECORD_SIZE`](#const-tls-max-record-size)
- [const `TLS_PREMASTER_SECRET_SIZE`](#const-tls-premaster-secret-size)
- [const `TLS_MASTER_SECRET_SIZE`](#const-tls-master-secret-size)
- [const `TLS_VERIFY_DATA_SIZE`](#const-tls-verify-data-size)
- [const `SHA256_DIGEST_SIZE`](#const-sha256-digest-size)
- [function `buildAesSbox`](#function-buildaessbox)
- [function `writeSboxRow`](#function-writesboxrow)
- [function `aesRcon`](#function-aesrcon)
- [function `aesKeyExpansion`](#function-aeskeyexpansion)
- [function `xtime`](#function-xtime)
- [function `aesEncryptBlock`](#function-aesencryptblock)
- [function `gcmMultiply`](#function-gcmmultiply)
- [function `ghash`](#function-ghash)
- [function `ghashProcessBlocks`](#function-ghashprocessblocks)
- [function `aesCtrIncrement`](#function-aesctrincrement)
- [function `aesGcmEncrypt`](#function-aesgcmencrypt)
- [function `aesGcmDecrypt`](#function-aesgcmdecrypt)
- [function `tlsPrf`](#function-tlsprf)
- [function `sendTlsRecord`](#function-sendtlsrecord)
- [function `sendEncryptedRecord`](#function-sendencryptedrecord)
- [function `recvExact`](#function-recvexact)
- [function `recvTlsRecord`](#function-recvtlsrecord)
- [function `decryptReceivedRecord`](#function-decryptreceivedrecord)
- [function `buildClientHello`](#function-buildclienthello)
- [function `buildClientKeyExchange`](#function-buildclientkeyexchange)
- [function `buildChangeCipherSpec`](#function-buildchangecipherspec)
- [function `buildFinished`](#function-buildfinished)
- [function `parseServerCertificate`](#function-parseservercertificate)
- [function `deriveMasterSecret`](#function-derivemastersecret)
- [function `deriveKeyMaterial`](#function-derivekeymaterial)
- [class `TlsSocket`](#class-tlssocket)
  - [`TlsSocket()`](#TlsSocket)
  - [`performHandshake()`](#performHandshake)
  - [`send()`](#send)
  - [`receive()`](#receive)
  - [`sendRaw()`](#sendRaw)
  - [`receiveRaw()`](#receiveRaw)
  - [`close()`](#close)
  - [`destroy()`](#destroy)
  - [`isConnected()`](#isConnected)
  - [`getSocketFd()`](#getSocketFd)

## Imports

- `uranite.crypto.hmac`
  - `hmacSha256`
- `uranite.crypto.random`
  - `randomBytes`
- `uranite.crypto.rsa`
  - `RsaPublicKey`
  - `rsaEncryptPkcs1v15`
  - `rsaPublicKeyFromDer`
- `uranite.crypto.sha256`
  - `Sha256`
  - `sha256`
- `uranite.encoding.binary`
  - `copyBytes`
  - `readI16Be`
  - `writeI16Be`
- `uranite.encoding.der`
  - `DerElement`
  - `derIterateSequence`
  - `derParseElement`
  - `derParseSequence`
- `uranite.encoding.pem`
  - `PemBlock`
  - `pemDecode`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `stringLen`
  - `stringToPtr`
- `uranite.io.syscall`
  - `stringLen`
  - `stringToPtr`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`
- `uranite.net.socket`
  - `socketClose`
  - `socketRecv`
  - `socketSend`
- `uranite.net.tcp`
  - `TcpSocket`

## const `TLS_VERSION_10`

## const `TLS_VERSION_12`

## const `CONTENT_TYPE_CHANGE_CIPHER_SPEC`

## const `CONTENT_TYPE_ALERT`

## const `CONTENT_TYPE_HANDSHAKE`

## const `CONTENT_TYPE_APPLICATION_DATA`

## const `HANDSHAKE_CLIENT_HELLO`

## const `HANDSHAKE_SERVER_HELLO`

## const `HANDSHAKE_CERTIFICATE`

## const `HANDSHAKE_SERVER_HELLO_DONE`

## const `HANDSHAKE_CLIENT_KEY_EXCHANGE`

## const `HANDSHAKE_FINISHED`

## const `CIPHER_TLS_RSA_AES_128_GCM_SHA256`

## const `AES_BLOCK_SIZE`

## const `AES_128_KEY_SIZE`

## const `AES_128_ROUNDS`

## const `GCM_IV_SIZE`

## const `GCM_TAG_SIZE`

## const `GCM_EXPLICIT_NONCE_SIZE`

## const `TLS_RECORD_HEADER_SIZE`

## const `TLS_MAX_RECORD_SIZE`

## const `TLS_PREMASTER_SECRET_SIZE`

## const `TLS_MASTER_SECRET_SIZE`

## const `TLS_VERIFY_DATA_SIZE`

## const `SHA256_DIGEST_SIZE`

## function `buildAesSbox`

Allocate and initialize the AES forward S-box lookup table (256 bytes). Returns address of the allocated table. Caller must dealloc when done.

**Returns**: — I64:
Address of 256-byte S-box table.

**Complexity**:
- Time: `O(1)`
- Space: `O(256)`

### Methods

#### `function buildAesSbox(  ) -> I64`

Allocate and initialize the AES forward S-box lookup table (256 bytes). Returns address of the allocated table. Caller must dealloc when done.

**Returns**: — I64:
Address of 256-byte S-box table.

**Complexity**:
- Time: `O(1)`
- Space: `O(256)`

## function `writeSboxRow`

Write 16 S-box entries at the given row offset.

**Parameters**:

- `tableAddress` (`I64`)
- `Base address of S-box table.`
- `rowOffset` (`I64`)
- `Byte offset for this row` (`0, 16, 32, ...`)
- `b0..b15` (`I64`)
- `The 16 S-box values for this row.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function writeSboxRow( I64 tableAddress, I64 rowOffset, I64 b0, I64 b1, I64 b2, I64 b3, I64 b4, I64 b5, I64 b6, I64 b7, I64 b8, I64 b9, I64 b10, I64 b11, I64 b12, I64 b13, I64 b14, I64 b15 ) -> Void`

Write 16 S-box entries at the given row offset.

**Parameters**:

- `tableAddress` (`I64`)
- `Base address of S-box table.`
- `rowOffset` (`I64`)
- `Byte offset for this row` (`0, 16, 32, ...`)
- `b0..b15` (`I64`)
- `The 16 S-box values for this row.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `aesRcon`

Return the AES round constant (Rcon) for the given round index.

**Parameters**:

- `roundIndex` (`I64`)
- `Round index` (`1-10 for AES-128`)

**Returns**: — I64:
Round constant byte value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function aesRcon( I64 roundIndex ) -> I64`

Return the AES round constant (Rcon) for the given round index.

**Parameters**:

- `roundIndex` (`I64`)
- `Round index` (`1-10 for AES-128`)

**Returns**: — I64:
Round constant byte value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `aesKeyExpansion`

Expand a 16-byte AES-128 key into 11 round keys (176 bytes total). Each round key is 16 bytes. Stored sequentially at expandedKeyAddress.

**Parameters**:

- `keyAddress` (`I64`)
- `Address of 16-byte key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte output buffer.`

**Complexity**:
- Time: `O(1) (fixed 10 rounds)`
- Space: `O(1) (in-place expansion)`

### Methods

#### `function aesKeyExpansion( I64 keyAddress, I64 sboxAddress, I64 expandedKeyAddress ) -> Void`

Expand a 16-byte AES-128 key into 11 round keys (176 bytes total). Each round key is 16 bytes. Stored sequentially at expandedKeyAddress.

**Parameters**:

- `keyAddress` (`I64`)
- `Address of 16-byte key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte output buffer.`

**Complexity**:
- Time: `O(1) (fixed 10 rounds)`
- Space: `O(1) (in-place expansion)`

## function `xtime`

GF(2^8) multiplication by 2 (used in MixColumns). If high bit set, XOR with irreducible polynomial 0x1B.

**Parameters**:

- `value` (`I64`)
- `Byte value to multiply.`

**Returns**: — I64:
Result of multiplication by 2 in GF(2^8).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function xtime( I64 value ) -> I64`

GF(2^8) multiplication by 2 (used in MixColumns). If high bit set, XOR with irreducible polynomial 0x1B.

**Parameters**:

- `value` (`I64`)
- `Byte value to multiply.`

**Returns**: — I64:
Result of multiplication by 2 in GF(2^8).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `aesEncryptBlock`

Encrypt a single 16-byte block using AES-128. Operates on the state matrix in column-major order as per FIPS 197.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of 16-byte plaintext block.`
- `outputAddress` (`I64`)
- `Address of 16-byte output buffer.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`

**Complexity**:
- Time: `O(1) (fixed 10 rounds)`
- Space: `O(1) (16-byte state on stack)`

### Methods

#### `function aesEncryptBlock( I64 inputAddress, I64 outputAddress, I64 expandedKeyAddress, I64 sboxAddress ) -> Void`

Encrypt a single 16-byte block using AES-128. Operates on the state matrix in column-major order as per FIPS 197.

**Parameters**:

- `inputAddress` (`I64`)
- `Address of 16-byte plaintext block.`
- `outputAddress` (`I64`)
- `Address of 16-byte output buffer.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`

**Complexity**:
- Time: `O(1) (fixed 10 rounds)`
- Space: `O(1) (16-byte state on stack)`

## function `gcmMultiply`

Multiply two 128-bit values in GF(2^128) using the GHASH polynomial x^128 + x^7 + x^2 + x + 1 (0xE1... reflected). Uses the standard shift-and-XOR algorithm.

**Parameters**:

- `xAddress` (`I64`)
- `Address of first 16-byte operand.`
- `yAddress` (`I64`)
- `Address of second 16-byte operand.`
- `resultAddress` (`I64`)
- `Address of 16-byte result buffer.`

**Complexity**:
- Time: `O(128) bit operations`
- Space: `O(16) for temporary buffer`

### Methods

#### `function gcmMultiply( I64 xAddress, I64 yAddress, I64 resultAddress ) -> Void`

Multiply two 128-bit values in GF(2^128) using the GHASH polynomial x^128 + x^7 + x^2 + x + 1 (0xE1... reflected). Uses the standard shift-and-XOR algorithm.

**Parameters**:

- `xAddress` (`I64`)
- `Address of first 16-byte operand.`
- `yAddress` (`I64`)
- `Address of second 16-byte operand.`
- `resultAddress` (`I64`)
- `Address of 16-byte result buffer.`

**Complexity**:
- Time: `O(128) bit operations`
- Space: `O(16) for temporary buffer`

## function `ghash`

Compute GHASH over associated data and ciphertext. Appends 64-bit lengths of AAD and ciphertext at the end per RFC 5116.

**Parameters**:

- `hashKeyAddress` (`I64`)
- `Address of 16-byte hash key H = AES_K` (`0`)
- `aadAddress` (`I64`)
- `Address of additional authenticated data.`
- `aadLength` (`I64`)
- `Length of AAD in bytes.`
- `ciphertextAddress` (`I64`)
- `Address of ciphertext.`
- `ciphertextLength` (`I64`)
- `Length of ciphertext in bytes.`
- `outputAddress` (`I64`)
- `Address of 16-byte output buffer.`

**Complexity**:
- Time: `O((aadLength + ciphertextLength) / 16 * 128)`
- Space: `O(16)`

### Methods

#### `function ghash( I64 hashKeyAddress, I64 aadAddress, I64 aadLength, I64 ciphertextAddress, I64 ciphertextLength, I64 outputAddress ) -> Void`

Compute GHASH over associated data and ciphertext. Appends 64-bit lengths of AAD and ciphertext at the end per RFC 5116.

**Parameters**:

- `hashKeyAddress` (`I64`)
- `Address of 16-byte hash key H = AES_K` (`0`)
- `aadAddress` (`I64`)
- `Address of additional authenticated data.`
- `aadLength` (`I64`)
- `Length of AAD in bytes.`
- `ciphertextAddress` (`I64`)
- `Address of ciphertext.`
- `ciphertextLength` (`I64`)
- `Length of ciphertext in bytes.`
- `outputAddress` (`I64`)
- `Address of 16-byte output buffer.`

**Complexity**:
- Time: `O((aadLength + ciphertextLength) / 16 * 128)`
- Space: `O(16)`

## function `ghashProcessBlocks`

Process data blocks for GHASH accumulation. Handles padding of the final partial block with zeros.

**Parameters**:

- `hashKeyAddress` (`I64`)
- `Address of 16-byte hash key H.`
- `dataAddress` (`I64`)
- `Address of data to process.`
- `dataLength` (`I64`)
- `Length of data.`
- `accAddress` (`I64`)
- `Address of 16-byte accumulator` (`modified in place`)
- `blockAddress` (`I64`)
- `Address of 16-byte temporary block buffer.`
- `tempAddress` (`I64`)
- `Address of 16-byte temporary multiply buffer.`

**Complexity**:
- Time: `O(dataLength / 16 * 128)`
- Space: `O(1)`

### Methods

#### `function ghashProcessBlocks( I64 hashKeyAddress, I64 dataAddress, I64 dataLength, I64 accAddress, I64 blockAddress, I64 tempAddress ) -> Void`

Process data blocks for GHASH accumulation. Handles padding of the final partial block with zeros.

**Parameters**:

- `hashKeyAddress` (`I64`)
- `Address of 16-byte hash key H.`
- `dataAddress` (`I64`)
- `Address of data to process.`
- `dataLength` (`I64`)
- `Length of data.`
- `accAddress` (`I64`)
- `Address of 16-byte accumulator` (`modified in place`)
- `blockAddress` (`I64`)
- `Address of 16-byte temporary block buffer.`
- `tempAddress` (`I64`)
- `Address of 16-byte temporary multiply buffer.`

**Complexity**:
- Time: `O(dataLength / 16 * 128)`
- Space: `O(1)`

## function `aesCtrIncrement`

Increment the rightmost 32 bits of a 16-byte counter block (big-endian, wrapping at 2^32).

**Parameters**:

- `counterAddress` (`I64`)
- `Address of 16-byte counter block.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function aesCtrIncrement( I64 counterAddress ) -> Void`

Increment the rightmost 32 bits of a 16-byte counter block (big-endian, wrapping at 2^32).

**Parameters**:

- `counterAddress` (`I64`)
- `Address of 16-byte counter block.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `aesGcmEncrypt`

AES-128-GCM authenticated encryption. Encrypts plaintext using CTR mode and generates a 16-byte authentication tag via GHASH.

**Parameters**:

- `plaintextAddress` (`I64`)
- `Address of plaintext data.`
- `plaintextLength` (`I64`)
- `Length of plaintext in bytes.`
- `aadAddress` (`I64`)
- `Address of additional authenticated data.`
- `aadLength` (`I64`)
- `Length of AAD in bytes.`
- `ivAddress` (`I64`)
- `Address of 12-byte IV/nonce.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded AES key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte AES S-box.`
- `ciphertextAddress` (`I64`)
- `Address of output buffer` (`same size as plaintext`)
- `tagAddress` (`I64`)
- `Address of 16-byte output tag buffer.`

**Returns**: — I64:
Ciphertext length (same as plaintext length).

**Complexity**:
- Time: `O(n) where n = plaintextLength`
- Space: `O(1) beyond output buffers`

### Methods

#### `function aesGcmEncrypt( I64 plaintextAddress, I64 plaintextLength, I64 aadAddress, I64 aadLength, I64 ivAddress, I64 expandedKeyAddress, I64 sboxAddress, I64 ciphertextAddress, I64 tagAddress ) -> I64`

AES-128-GCM authenticated encryption. Encrypts plaintext using CTR mode and generates a 16-byte authentication tag via GHASH.

**Parameters**:

- `plaintextAddress` (`I64`)
- `Address of plaintext data.`
- `plaintextLength` (`I64`)
- `Length of plaintext in bytes.`
- `aadAddress` (`I64`)
- `Address of additional authenticated data.`
- `aadLength` (`I64`)
- `Length of AAD in bytes.`
- `ivAddress` (`I64`)
- `Address of 12-byte IV/nonce.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded AES key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte AES S-box.`
- `ciphertextAddress` (`I64`)
- `Address of output buffer` (`same size as plaintext`)
- `tagAddress` (`I64`)
- `Address of 16-byte output tag buffer.`

**Returns**: — I64:
Ciphertext length (same as plaintext length).

**Complexity**:
- Time: `O(n) where n = plaintextLength`
- Space: `O(1) beyond output buffers`

## function `aesGcmDecrypt`

AES-128-GCM authenticated decryption. Verifies the authentication tag and decrypts ciphertext. Returns -1 if tag verification fails.

**Parameters**:

- `ciphertextAddress` (`I64`)
- `Address of ciphertext.`
- `ciphertextLength` (`I64`)
- `Length of ciphertext.`
- `aadAddress` (`I64`)
- `Address of AAD.`
- `aadLength` (`I64`)
- `Length of AAD.`
- `ivAddress` (`I64`)
- `Address of 12-byte IV.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`
- `tagAddress` (`I64`)
- `Address of 16-byte expected tag.`
- `plaintextAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Plaintext length on success, -1 on tag mismatch.

**Complexity**:
- Time: `O(n) where n = ciphertextLength`
- Space: `O(16) for computed tag`

### Methods

#### `function aesGcmDecrypt( I64 ciphertextAddress, I64 ciphertextLength, I64 aadAddress, I64 aadLength, I64 ivAddress, I64 expandedKeyAddress, I64 sboxAddress, I64 tagAddress, I64 plaintextAddress ) -> I64`

AES-128-GCM authenticated decryption. Verifies the authentication tag and decrypts ciphertext. Returns -1 if tag verification fails.

**Parameters**:

- `ciphertextAddress` (`I64`)
- `Address of ciphertext.`
- `ciphertextLength` (`I64`)
- `Length of ciphertext.`
- `aadAddress` (`I64`)
- `Address of AAD.`
- `aadLength` (`I64`)
- `Length of AAD.`
- `ivAddress` (`I64`)
- `Address of 12-byte IV.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`
- `tagAddress` (`I64`)
- `Address of 16-byte expected tag.`
- `plaintextAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Plaintext length on success, -1 on tag mismatch.

**Complexity**:
- Time: `O(n) where n = ciphertextLength`
- Space: `O(16) for computed tag`

## function `tlsPrf`

TLS 1.2 PRF using HMAC-SHA-256: P_SHA256(secret, label + seed).

A(0) = label + seed A(i) = HMAC_SHA256(secret, A(i-1)) PRF output = HMAC_SHA256(secret, A(1) + label + seed) + HMAC_SHA256(secret, A(2) + label + seed) + ...

**Parameters**:

- `secretAddress` (`I64`)
- `Address of PRF secret.`
- `secretLength` (`I64`)
- `Length of secret.`
- `labelAddress` (`I64`)
- `Address of label bytes` (`e.g. "master secret"`)
- `labelLength` (`I64`)
- `Length of label.`
- `seedAddress` (`I64`)
- `Address of seed bytes.`
- `seedLength` (`I64`)
- `Length of seed.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputLength` (`I64`)
- `Number of bytes to generate.`

**Complexity**:
- Time: `O(outputLength * HMAC cost)`
- Space: `O(label + seed + SHA256 digest)`

### Methods

#### `function tlsPrf( I64 secretAddress, I64 secretLength, I64 labelAddress, I64 labelLength, I64 seedAddress, I64 seedLength, I64 outputAddress, I64 outputLength ) -> Void`

TLS 1.2 PRF using HMAC-SHA-256: P_SHA256(secret, label + seed).

A(0) = label + seed A(i) = HMAC_SHA256(secret, A(i-1)) PRF output = HMAC_SHA256(secret, A(1) + label + seed) + HMAC_SHA256(secret, A(2) + label + seed) + ...

**Parameters**:

- `secretAddress` (`I64`)
- `Address of PRF secret.`
- `secretLength` (`I64`)
- `Length of secret.`
- `labelAddress` (`I64`)
- `Address of label bytes` (`e.g. "master secret"`)
- `labelLength` (`I64`)
- `Length of label.`
- `seedAddress` (`I64`)
- `Address of seed bytes.`
- `seedLength` (`I64`)
- `Length of seed.`
- `outputAddress` (`I64`)
- `Address of output buffer.`
- `outputLength` (`I64`)
- `Number of bytes to generate.`

**Complexity**:
- Time: `O(outputLength * HMAC cost)`
- Space: `O(label + seed + SHA256 digest)`

## function `sendTlsRecord`

Send an unencrypted TLS record (used during handshake before ChangeCipherSpec).

**Parameters**:

- `socketFd` (`I64`)
- `TCP socket file descriptor.`
- `contentType` (`I64`)
- `TLS content type` (`20-23`)
- `dataAddress` (`I64`)
- `Address of record payload.`
- `dataLength` (`I64`)
- `Length of payload.`

**Returns**: — I64:
Total bytes sent including header.

**Complexity**:
- Time: `O(dataLength)`
- Space: `O(dataLength + 5)`

### Methods

#### `function sendTlsRecord( I64 socketFd, I64 contentType, I64 dataAddress, I64 dataLength ) -> I64`

Send an unencrypted TLS record (used during handshake before ChangeCipherSpec).

**Parameters**:

- `socketFd` (`I64`)
- `TCP socket file descriptor.`
- `contentType` (`I64`)
- `TLS content type` (`20-23`)
- `dataAddress` (`I64`)
- `Address of record payload.`
- `dataLength` (`I64`)
- `Length of payload.`

**Returns**: — I64:
Total bytes sent including header.

**Complexity**:
- Time: `O(dataLength)`
- Space: `O(dataLength + 5)`

## function `sendEncryptedRecord`

Send an AES-128-GCM encrypted TLS record. Per RFC 5288, the nonce is implicit IV (4 bytes) + explicit nonce (8 bytes, = sequence number).

**Parameters**:

- `socketFd` (`I64`)
- `TCP socket file descriptor.`
- `contentType` (`I64`)
- `TLS content type.`
- `dataAddress` (`I64`)
- `Address of plaintext data.`
- `dataLength` (`I64`)
- `Length of plaintext.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded AES key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`
- `implicitIvAddress` (`I64`)
- `Address of 4-byte implicit IV` (`from key material`)
- `sequenceNumber` (`I64`)
- `64-bit sequence number for this record.`

**Returns**: — I64:
Total bytes sent.

**Complexity**:
- Time: `O(dataLength)`
- Space: `O(dataLength + record overhead)`

### Methods

#### `function sendEncryptedRecord( I64 socketFd, I64 contentType, I64 dataAddress, I64 dataLength, I64 expandedKeyAddress, I64 sboxAddress, I64 implicitIvAddress, I64 sequenceNumber ) -> I64`

Send an AES-128-GCM encrypted TLS record. Per RFC 5288, the nonce is implicit IV (4 bytes) + explicit nonce (8 bytes, = sequence number).

**Parameters**:

- `socketFd` (`I64`)
- `TCP socket file descriptor.`
- `contentType` (`I64`)
- `TLS content type.`
- `dataAddress` (`I64`)
- `Address of plaintext data.`
- `dataLength` (`I64`)
- `Length of plaintext.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded AES key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`
- `implicitIvAddress` (`I64`)
- `Address of 4-byte implicit IV` (`from key material`)
- `sequenceNumber` (`I64`)
- `64-bit sequence number for this record.`

**Returns**: — I64:
Total bytes sent.

**Complexity**:
- Time: `O(dataLength)`
- Space: `O(dataLength + record overhead)`

## function `recvExact`

Receive exactly totalLength bytes from the socket, looping until all bytes are read or an error/EOF occurs.

**Parameters**:

- `socketFd` (`I64`)
- `TCP socket file descriptor.`
- `bufferAddress` (`I64`)
- `Address of receive buffer.`
- `totalLength` (`I64`)
- `Exact number of bytes to receive.`

**Returns**: — I64:
totalLength on success, -1 on error/premature EOF.

**Complexity**:
- Time: `O(totalLength)`
- Space: `O(1)`

### Methods

#### `function recvExact( I64 socketFd, I64 bufferAddress, I64 totalLength ) -> I64`

Receive exactly totalLength bytes from the socket, looping until all bytes are read or an error/EOF occurs.

**Parameters**:

- `socketFd` (`I64`)
- `TCP socket file descriptor.`
- `bufferAddress` (`I64`)
- `Address of receive buffer.`
- `totalLength` (`I64`)
- `Exact number of bytes to receive.`

**Returns**: — I64:
totalLength on success, -1 on error/premature EOF.

**Complexity**:
- Time: `O(totalLength)`
- Space: `O(1)`

## function `recvTlsRecord`

Receive a single TLS record. Reads the 5-byte header, then the payload.

**Parameters**:

- `socketFd` (`I64`)
- `TCP socket file descriptor.`
- `contentTypeOut` (`I64`)
- `Address where the content type byte will be stored.`
- `payloadAddress` (`I64`)
- `Address of payload output buffer.`
- `payloadCapacity` (`I64`)
- `Maximum payload capacity.`

**Returns**: — I64:
Payload length on success, -1 on error.

**Complexity**:
- Time: `O(payload length)`
- Space: `O(5) for header`

### Methods

#### `function recvTlsRecord( I64 socketFd, I64 contentTypeOut, I64 payloadAddress, I64 payloadCapacity ) -> I64`

Receive a single TLS record. Reads the 5-byte header, then the payload.

**Parameters**:

- `socketFd` (`I64`)
- `TCP socket file descriptor.`
- `contentTypeOut` (`I64`)
- `Address where the content type byte will be stored.`
- `payloadAddress` (`I64`)
- `Address of payload output buffer.`
- `payloadCapacity` (`I64`)
- `Maximum payload capacity.`

**Returns**: — I64:
Payload length on success, -1 on error.

**Complexity**:
- Time: `O(payload length)`
- Space: `O(5) for header`

## function `decryptReceivedRecord`

Decrypt a received AES-128-GCM TLS record. The payload contains: explicit_nonce (8 bytes) + ciphertext + tag (16 bytes).

**Parameters**:

- `payloadAddress` (`I64`)
- `Address of encrypted record payload` (`after TLS header`)
- `payloadLength` (`I64`)
- `Length of encrypted payload.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded AES key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`
- `implicitIvAddress` (`I64`)
- `Address of 4-byte implicit IV.`
- `sequenceNumber` (`I64`)
- `Expected sequence number.`
- `contentType` (`I64`)
- `TLS content type for AAD construction.`
- `plaintextAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Plaintext length on success, -1 on authentication failure.

**Complexity**:
- Time: `O(payloadLength)`
- Space: `O(13) for AAD`

### Methods

#### `function decryptReceivedRecord( I64 payloadAddress, I64 payloadLength, I64 expandedKeyAddress, I64 sboxAddress, I64 implicitIvAddress, I64 sequenceNumber, I64 contentType, I64 plaintextAddress ) -> I64`

Decrypt a received AES-128-GCM TLS record. The payload contains: explicit_nonce (8 bytes) + ciphertext + tag (16 bytes).

**Parameters**:

- `payloadAddress` (`I64`)
- `Address of encrypted record payload` (`after TLS header`)
- `payloadLength` (`I64`)
- `Length of encrypted payload.`
- `expandedKeyAddress` (`I64`)
- `Address of 176-byte expanded AES key.`
- `sboxAddress` (`I64`)
- `Address of 256-byte S-box.`
- `implicitIvAddress` (`I64`)
- `Address of 4-byte implicit IV.`
- `sequenceNumber` (`I64`)
- `Expected sequence number.`
- `contentType` (`I64`)
- `TLS content type for AAD construction.`
- `plaintextAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Plaintext length on success, -1 on authentication failure.

**Complexity**:
- Time: `O(payloadLength)`
- Space: `O(13) for AAD`

## function `buildClientHello`

Build a TLS 1.2 ClientHello handshake message. Offers a single cipher suite (TLS_RSA_WITH_AES_128_GCM_SHA256) and no extensions.

**Parameters**:

- `clientRandomAddress` (`I64`)
- `Address of 32-byte client random.`
- `outputAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Total handshake message length (including header).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function buildClientHello( I64 clientRandomAddress, I64 outputAddress ) -> I64`

Build a TLS 1.2 ClientHello handshake message. Offers a single cipher suite (TLS_RSA_WITH_AES_128_GCM_SHA256) and no extensions.

**Parameters**:

- `clientRandomAddress` (`I64`)
- `Address of 32-byte client random.`
- `outputAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Total handshake message length (including header).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `buildClientKeyExchange`

Build a ClientKeyExchange handshake message containing the RSA-encrypted premaster secret.

**Parameters**:

- `encryptedPmsAddress` (`I64`)
- `Address of RSA-encrypted premaster secret.`
- `encryptedPmsLength` (`I64`)
- `Length of encrypted premaster secret.`
- `outputAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Total message length.

**Complexity**:
- Time: `O(encryptedPmsLength)`
- Space: `O(1)`

### Methods

#### `function buildClientKeyExchange( I64 encryptedPmsAddress, I64 encryptedPmsLength, I64 outputAddress ) -> I64`

Build a ClientKeyExchange handshake message containing the RSA-encrypted premaster secret.

**Parameters**:

- `encryptedPmsAddress` (`I64`)
- `Address of RSA-encrypted premaster secret.`
- `encryptedPmsLength` (`I64`)
- `Length of encrypted premaster secret.`
- `outputAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Total message length.

**Complexity**:
- Time: `O(encryptedPmsLength)`
- Space: `O(1)`

## function `buildChangeCipherSpec`

Build a ChangeCipherSpec message (single byte: 0x01).

**Parameters**:

- `outputAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Message length (1).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function buildChangeCipherSpec( I64 outputAddress ) -> I64`

Build a ChangeCipherSpec message (single byte: 0x01).

**Parameters**:

- `outputAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Message length (1).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `buildFinished`

Build a Finished handshake message containing the 12-byte verify_data = PRF(master_secret, "client finished", Hash(messages)).

**Parameters**:

- `masterSecretAddress` (`I64`)
- `Address of 48-byte master secret.`
- `handshakeHashAddress` (`I64`)
- `Address of 32-byte SHA-256 hash of all handshake messages.`
- `outputAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Total message length (4 + 12 = 16).

**Complexity**:
- Time: `O(PRF cost)`
- Space: `O(12)`

### Methods

#### `function buildFinished( I64 masterSecretAddress, I64 handshakeHashAddress, I64 outputAddress ) -> I64`

Build a Finished handshake message containing the 12-byte verify_data = PRF(master_secret, "client finished", Hash(messages)).

**Parameters**:

- `masterSecretAddress` (`I64`)
- `Address of 48-byte master secret.`
- `handshakeHashAddress` (`I64`)
- `Address of 32-byte SHA-256 hash of all handshake messages.`
- `outputAddress` (`I64`)
- `Address of output buffer.`

**Returns**: — I64:
Total message length (4 + 12 = 16).

**Complexity**:
- Time: `O(PRF cost)`
- Space: `O(12)`

## function `parseServerCertificate`

Parse the server's Certificate handshake message and extract the RSA public key from the first certificate in the chain.

**Parameters**:

- `payloadAddress` (`I64`)
- `Address of Certificate message body` (`after handshake header`)
- `payloadLength` (`I64`)
- `Length of message body.`

**Returns**: — RsaPublicKey:
Server's RSA public key.

**Complexity**:
- Time: `O(certificate size)`
- Space: `O(key size)`

### Methods

#### `function parseServerCertificate( I64 payloadAddress, I64 payloadLength ) -> RsaPublicKey`

Parse the server's Certificate handshake message and extract the RSA public key from the first certificate in the chain.

**Parameters**:

- `payloadAddress` (`I64`)
- `Address of Certificate message body` (`after handshake header`)
- `payloadLength` (`I64`)
- `Length of message body.`

**Returns**: — RsaPublicKey:
Server's RSA public key.

**Complexity**:
- Time: `O(certificate size)`
- Space: `O(key size)`

## function `deriveMasterSecret`

Derive the TLS 1.2 master secret: master_secret = PRF(pre_master_secret, "master secret", ClientHello.random + ServerHello.random)

**Parameters**:

- `premasterAddress` (`I64`)
- `Address of 48-byte premaster secret.`
- `clientRandomAddress` (`I64`)
- `Address of 32-byte client random.`
- `serverRandomAddress` (`I64`)
- `Address of 32-byte server random.`
- `masterSecretAddress` (`I64`)
- `Address of 48-byte output buffer.`

**Complexity**:
- Time: `O(PRF cost)`
- Space: `O(64)`

### Methods

#### `function deriveMasterSecret( I64 premasterAddress, I64 clientRandomAddress, I64 serverRandomAddress, I64 masterSecretAddress ) -> Void`

Derive the TLS 1.2 master secret: master_secret = PRF(pre_master_secret, "master secret", ClientHello.random + ServerHello.random)

**Parameters**:

- `premasterAddress` (`I64`)
- `Address of 48-byte premaster secret.`
- `clientRandomAddress` (`I64`)
- `Address of 32-byte client random.`
- `serverRandomAddress` (`I64`)
- `Address of 32-byte server random.`
- `masterSecretAddress` (`I64`)
- `Address of 48-byte output buffer.`

**Complexity**:
- Time: `O(PRF cost)`
- Space: `O(64)`

## function `deriveKeyMaterial`

Derive TLS 1.2 key material from master secret: key_block = PRF(master_secret, "key expansion", server_random + client_random)

For AES_128_GCM_SHA256: client_write_key (16 bytes) + server_write_key (16 bytes) + client_write_IV (4 bytes) + server_write_IV (4 bytes) = 40 bytes
 Note: GCM has no MAC keys (authentication is built into AEAD).

**Parameters**:

- `masterSecretAddress` (`I64`)
- `Address of 48-byte master secret.`
- `serverRandomAddress` (`I64`)
- `Address of 32-byte server random.`
- `clientRandomAddress` (`I64`)
- `Address of 32-byte client random.`
- `clientWriteKeyAddress` (`I64`)
- `Address of 16-byte output for client write key.`
- `serverWriteKeyAddress` (`I64`)
- `Address of 16-byte output for server write key.`
- `clientWriteIvAddress` (`I64`)
- `Address of 4-byte output for client write IV.`
- `serverWriteIvAddress` (`I64`)
- `Address of 4-byte output for server write IV.`

**Complexity**:
- Time: `O(PRF cost)`
- Space: `O(64 + 40)`

### Methods

#### `function deriveKeyMaterial( I64 masterSecretAddress, I64 serverRandomAddress, I64 clientRandomAddress, I64 clientWriteKeyAddress, I64 serverWriteKeyAddress, I64 clientWriteIvAddress, I64 serverWriteIvAddress ) -> Void`

Derive TLS 1.2 key material from master secret: key_block = PRF(master_secret, "key expansion", server_random + client_random)

For AES_128_GCM_SHA256: client_write_key (16 bytes) + server_write_key (16 bytes) + client_write_IV (4 bytes) + server_write_IV (4 bytes) = 40 bytes
 Note: GCM has no MAC keys (authentication is built into AEAD).

**Parameters**:

- `masterSecretAddress` (`I64`)
- `Address of 48-byte master secret.`
- `serverRandomAddress` (`I64`)
- `Address of 32-byte server random.`
- `clientRandomAddress` (`I64`)
- `Address of 32-byte client random.`
- `clientWriteKeyAddress` (`I64`)
- `Address of 16-byte output for client write key.`
- `serverWriteKeyAddress` (`I64`)
- `Address of 16-byte output for server write key.`
- `clientWriteIvAddress` (`I64`)
- `Address of 4-byte output for client write IV.`
- `serverWriteIvAddress` (`I64`)
- `Address of 4-byte output for server write IV.`

**Complexity**:
- Time: `O(PRF cost)`
- Space: `O(64 + 40)`

## class `TlsSocket`

TLS 1.2 client socket wrapping a raw TCP file descriptor. Performs full handshake on construction, then provides encrypted send/receive.

Cipher suite: TLS_RSA_WITH_AES_128_GCM_SHA256 (0x009C). Key exchange: RSA (PKCS#1 v1.5). Record encryption: AES-128-GCM. PRF: HMAC-SHA-256.

### Fields

| Name | Type | Access |
|------|------|--------|
| `socketFd` | `I64` | protect |
| `clientExpandedKeyAddress` | `I64` | protect |
| `serverExpandedKeyAddress` | `I64` | protect |
| `sboxAddress` | `I64` | protect |
| `clientIvAddress` | `I64` | protect |
| `serverIvAddress` | `I64` | protect |
| `masterSecretAddress` | `I64` | protect |
| `clientSequenceNumber` | `I64` | protect |
| `serverSequenceNumber` | `I64` | protect |
| `handshakeComplete` | `Boolean` | protect |

### Methods

#### `function TlsSocket( self, TcpSocket socket ) -> Void`

Create a TLS socket wrapping an already-connected TcpSocket. Does NOT perform the handshake — call performHandshake() after construction.

**Parameters**:

- `socket` (`TcpSocket`)
- `A connected TcpSocket instance. The TlsSocket takes`
- `ownership of the underlying file descriptor.`

**Complexity**:
- Time: `O(1)`
- Space: `O(AES key schedule + S-box + IVs)`

#### `function TlsSocket( self, I64 socketFd ) -> Void`

Create a TLS socket over a raw file descriptor. Internal use only — prefer the TcpSocket-accepting constructor for public API.

**Parameters**:

- `socketFd` (`I64`)
- `Connected TCP socket file descriptor.`

**Complexity**:
- Time: `O(1)`
- Space: `O(AES key schedule + S-box + IVs)`

#### `function performHandshake( self ) -> I64`

Execute the full TLS 1.2 handshake: 1. Send ClientHello 2. Receive ServerHello (extract server random) 3. Receive Certificate (extract RSA public key) 4. Receive ServerHelloDone 5. Send ClientKeyExchange (RSA-encrypted premaster secret) 6. Send ChangeCipherSpec 7. Send Finished (encrypted) 8. Receive server ChangeCipherSpec 9. Receive server Finished (verify)

**Returns**: — I64:
0 on success, -1 on failure.

**Complexity**:
- Time: `O(RSA modexp + AES key expansion)`
- Space: `O(certificate chain + key material)`

#### `function send( self, String data ) -> I64`

Send a string over the encrypted TLS connection. The string is automatically converted to bytes and fragmented into TLS records if needed.

**Parameters**:

- `data` (`String`)
- `The data to send.`

**Returns**: — I64:
Total bytes of plaintext sent, or -1 if not connected.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(min(n, TLS_MAX_RECORD_SIZE))`

#### `function receive( self, I64 maxLength ) -> String`

Receive and decrypt application data from the TLS connection, returning the result as a String.

**Parameters**:

- `maxLength` (`I64`)
- `Maximum number of bytes to receive.`

**Returns**: — String:
Decrypted data as a string, or empty string on
connection close or error.

**Complexity**:
- Time: `O(received data length)`
- Space: `O(maxLength)`

#### `function sendRaw( self, I64 dataAddress, I64 dataLength ) -> I64`

Send raw bytes over the encrypted TLS connection. Use send(String) for ergonomic API; this method is for callers needing byte-level control.

**Parameters**:

- `dataAddress` (`I64`)
- `Address of data to send.`
- `dataLength` (`I64`)
- `Length of data.`

**Returns**: — I64:
Total bytes of plaintext sent, or -1 if not connected.

**Complexity**:
- Time: `O(dataLength)`
- Space: `O(min(dataLength, TLS_MAX_RECORD_SIZE))`

#### `function receiveRaw( self, I64 outputAddress, I64 outputCapacity ) -> I64`

Receive and decrypt raw bytes from the TLS connection. Use receive(I64) for ergonomic API; this method is for callers needing byte-level control.

**Parameters**:

- `outputAddress` (`I64`)
- `Address of output buffer for decrypted data.`
- `outputCapacity` (`I64`)
- `Maximum bytes to receive.`

**Returns**: — I64:
Decrypted data length, 0 on connection close, -1 on error.

**Complexity**:
- Time: `O(received data length)`
- Space: `O(TLS_MAX_RECORD_SIZE) for receive buffer`

#### `function close( self ) -> Void`

Close the TLS connection. Sends a close_notify alert (unencrypted for simplicity) and closes the underlying socket.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release all cryptographic material and internal buffers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isConnected( self ) -> Boolean`

Return whether the TLS handshake has completed.

**Returns**: — Boolean:
True if handshake is complete and connection is active.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getSocketFd( self ) -> I64`

Return the underlying TCP socket file descriptor.

**Returns**: — I64:
File descriptor.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

