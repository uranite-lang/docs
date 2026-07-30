# uranite.encoding.base64

## Table of Contents

- [Imports](#imports)
- [const `BASE64_PAD`](#const-base64-pad)
- [const `encodeTableAddress`](#const-encodetableaddress)
- [const `decodeTableAddress`](#const-decodetableaddress)
- [const `tablesInitialized`](#const-tablesinitialized)
- [class `Base64Error`](#class-base64error)
  - [`Base64Error()`](#Base64Error)
- [function `initializeBase64Tables`](#function-initializebase64tables)
  - [`initializeBase64Tables()`](#initializeBase64Tables)
- [function `base64EncodedLength`](#function-base64encodedlength)
  - [`base64EncodedLength()`](#base64EncodedLength)
- [function `base64Encode`](#function-base64encode)
  - [`base64Encode()`](#base64Encode)
- [function `base64DecodedLength`](#function-base64decodedlength)
  - [`base64DecodedLength()`](#base64DecodedLength)
- [function `base64Decode`](#function-base64decode)
  - [`base64Decode()`](#base64Decode)
- [function `base64EncodeString`](#function-base64encodestring)
  - [`base64EncodeString()`](#base64EncodeString)
- [function `base64DecodeString`](#function-base64decodestring)
  - [`base64DecodeString()`](#base64DecodeString)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `readByteAt`
  - `readI64At`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
  - `writeI64At`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `BASE64_PAD`

ASCII code for '=' padding character.

## const `encodeTableAddress`

Lazily initialized 64-byte encode lookup table (index -> ASCII char).

## const `decodeTableAddress`

Lazily initialized 256-byte decode lookup table (ASCII char -> 6-bit value).

## const `tablesInitialized`

Flag indicating whether encode/decode tables have been computed.

## class `Base64Error`

**Extends**: `Error`

Raised when Base64 decoding encounters invalid input such as illegal characters or incorrect padding.

### Methods

#### `function Base64Error( self, String message, I64 code, ?Error cause ) -> Void`

Construct a Base64 decoding error.

**Parameters**:

- `message` (`String`)
- `Description of the decoding failure.`
- `code` (`I64`)
- `Numeric error code` (`0 for general decode errors`)
- `cause` (`?Error`)
- `Optional underlying error.`

## function `initializeBase64Tables`

Build the 64-byte encode table mapping 6-bit indices to ASCII characters (A-Z, a-z, 0-9, +, /) and the 256-byte decode table mapping ASCII characters back to 6-bit values. Invalid decode entries are set to 255.

**Complexity**:
- Time: `O(1) (constant 320 iterations)`
- Space: `O(1) (320 bytes allocated)`

### Methods

#### `function initializeBase64Tables(  ) -> Void`

Build the 64-byte encode table mapping 6-bit indices to ASCII characters (A-Z, a-z, 0-9, +, /) and the 256-byte decode table mapping ASCII characters back to 6-bit values. Invalid decode entries are set to 255.

**Complexity**:
- Time: `O(1) (constant 320 iterations)`
- Space: `O(1) (320 bytes allocated)`

## function `base64EncodedLength`

Calculate the length of the Base64-encoded output for a given input length, including padding.

**Parameters**:

- `inputLength` (`I64`)
- `Number of bytes to encode.`

**Returns**: — I64:
Length of the Base64-encoded string in bytes.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function base64EncodedLength( I64 inputLength ) -> I64`

Calculate the length of the Base64-encoded output for a given input length, including padding.

**Parameters**:

- `inputLength` (`I64`)
- `Number of bytes to encode.`

**Returns**: — I64:
Length of the Base64-encoded string in bytes.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `base64Encode`

Encode raw binary data into Base64 ASCII at the output address. The output buffer must be at least base64EncodedLength(dataLength) bytes. Output is padded with '=' to a multiple of 4 characters.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address of the input data.`
- `dataLength` (`I64`)
- `Number of input bytes to encode.`
- `outputAddress` (`I64`)
- `Base address of the output buffer.`

**Returns**: — I64:
Number of Base64 characters written.

**Complexity**:
- Time: `O(n) where n is dataLength`
- Space: `O(1)`

### Methods

#### `function base64Encode( I64 dataAddress, I64 dataLength, I64 outputAddress ) -> I64`

Encode raw binary data into Base64 ASCII at the output address. The output buffer must be at least base64EncodedLength(dataLength) bytes. Output is padded with '=' to a multiple of 4 characters.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address of the input data.`
- `dataLength` (`I64`)
- `Number of input bytes to encode.`
- `outputAddress` (`I64`)
- `Base address of the output buffer.`

**Returns**: — I64:
Number of Base64 characters written.

**Complexity**:
- Time: `O(n) where n is dataLength`
- Space: `O(1)`

## function `base64DecodedLength`

Calculate the number of raw bytes that a Base64-encoded string will decode to, accounting for padding characters.

**Parameters**:

- `encodedAddress` (`I64`)
- `Base address of the Base64-encoded string.`
- `encodedLength` (`I64`)
- `Length of the encoded string in bytes.`

**Returns**: — I64:
Number of bytes in the decoded output.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function base64DecodedLength( I64 encodedAddress, I64 encodedLength ) -> I64`

Calculate the number of raw bytes that a Base64-encoded string will decode to, accounting for padding characters.

**Parameters**:

- `encodedAddress` (`I64`)
- `Base address of the Base64-encoded string.`
- `encodedLength` (`I64`)
- `Length of the encoded string in bytes.`

**Returns**: — I64:
Number of bytes in the decoded output.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `base64Decode`

Decode a Base64-encoded string into raw bytes at the output address. The output buffer must be at least base64DecodedLength bytes.

**Parameters**:

- `encodedAddress` (`I64`)
- `Base address of the Base64-encoded input.`
- `encodedLength` (`I64`)
- `Length of the encoded input in bytes.`
- `outputAddress` (`I64`)
- `Base address of the output buffer.`

**Returns**: `I64` — Number of decoded bytes written.

**Raises**:

- `Base64Error` → `Error` — When an invalid character is encountered in the input.

**Complexity**:
- Time: `O(n) where n is encodedLength`
- Space: `O(1)`

### Methods

#### `function base64Decode( I64 encodedAddress, I64 encodedLength, I64 outputAddress ) -> I64`

Decode a Base64-encoded string into raw bytes at the output address. The output buffer must be at least base64DecodedLength bytes.

**Parameters**:

- `encodedAddress` (`I64`)
- `Base address of the Base64-encoded input.`
- `encodedLength` (`I64`)
- `Length of the encoded input in bytes.`
- `outputAddress` (`I64`)
- `Base address of the output buffer.`

**Returns**: `I64` — Number of decoded bytes written.

**Raises**:

- `Base64Error` → `Error` — When an invalid character is encountered in the input.

**Complexity**:
- Time: `O(n) where n is encodedLength`
- Space: `O(1)`

## function `base64EncodeString`

Encode a string to Base64 and return the result as a string.

**Parameters**:

- `data` (`String`)
- `The input data to encode.`

**Returns**: — String:
Base64-encoded string with padding.

**Complexity**:
- Time: `O(n) where n is input length`
- Space: `O(n)`

### Methods

#### `function base64EncodeString( String data ) -> String`

Encode a string to Base64 and return the result as a string.

**Parameters**:

- `data` (`String`)
- `The input data to encode.`

**Returns**: — String:
Base64-encoded string with padding.

**Complexity**:
- Time: `O(n) where n is input length`
- Space: `O(n)`

## function `base64DecodeString`

Decode a Base64-encoded string and return the raw bytes as a string.

**Parameters**:

- `encoded` (`String`)
- `The Base64-encoded input string.`

**Returns**: `String` — Decoded bytes as a string.

**Raises**:

- `Base64Error` → `Error` — When the input contains invalid Base64 characters.

**Complexity**:
- Time: `O(n) where n is encoded length`
- Space: `O(n)`

### Methods

#### `function base64DecodeString( String encoded ) -> String`

Decode a Base64-encoded string and return the raw bytes as a string.

**Parameters**:

- `encoded` (`String`)
- `The Base64-encoded input string.`

**Returns**: `String` — Decoded bytes as a string.

**Raises**:

- `Base64Error` → `Error` — When the input contains invalid Base64 characters.

**Complexity**:
- Time: `O(n) where n is encoded length`
- Space: `O(n)`

