# uranite.encoding.pem

## Table of Contents

- [Imports](#imports)
- [const `PEM_BEGIN_MARKER_LENGTH`](#const-pem-begin-marker-length)
- [const `PEM_END_MARKER_LENGTH`](#const-pem-end-marker-length)
- [const `PEM_MAX_DECODED_SIZE`](#const-pem-max-decoded-size)
- [class `PemBlock`](#class-pemblock)
  - [`PemBlock()`](#PemBlock)
  - [`destroy()`](#destroy)
- [function `pemDecode`](#function-pemdecode)
  - [`pemDecode()`](#pemDecode)
- [function `pemDecodeString`](#function-pemdecodestring)
  - [`pemDecodeString()`](#pemDecodeString)
- [function `findBeginMarker`](#function-findbeginmarker)
- [function `findEndMarker`](#function-findendmarker)

## Imports

- `uranite.encoding.base64`
  - `base64Decode`
- `uranite.encoding.binary`
  - `copyBytes`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `PEM_BEGIN_MARKER_LENGTH`

## const `PEM_END_MARKER_LENGTH`

## const `PEM_MAX_DECODED_SIZE`

## class `PemBlock`

A decoded PEM block containing the raw DER bytes and the PEM label (e.g. "CERTIFICATE", "RSA PRIVATE KEY").

### Fields

| Name | Type | Access |
|------|------|--------|
| `derAddress` | `I64` | public |
| `derLength` | `I64` | public |
| `labelAddress` | `I64` | public |
| `labelLength` | `I64` | public |

### Methods

#### `function PemBlock( self, I64 derAddress, I64 derLength, I64 labelAddress, I64 labelLength ) -> Void`

**Parameters**:

- `derAddress` (`I64`)
- `Address of decoded DER bytes.`
- `derLength` (`I64`)
- `Length of DER data.`
- `labelAddress` (`I64`)
- `Address of label string bytes.`
- `labelLength` (`I64`)
- `Length of label string.`

#### `function destroy( self ) -> Void`

Release DER and label storage.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `pemDecode`

Decode the first PEM block found in the input. Extracts the base64 content between BEGIN and END markers, strips whitespace and newlines, and base64-decodes to DER bytes.

**Parameters**:

- `pemAddress` (`I64`)
- `Address of PEM-encoded text.`
- `pemLength` (`I64`)
- `Length of PEM text.`

**Returns**: — PemBlock:
Decoded PEM block with DER bytes and label.

**Complexity**:
- Time: `O(n) where n is pemLength`
- Space: `O(n) for base64 content + decoded DER`

### Methods

#### `function pemDecode( I64 pemAddress, I64 pemLength ) -> PemBlock`

Decode the first PEM block found in the input. Extracts the base64 content between BEGIN and END markers, strips whitespace and newlines, and base64-decodes to DER bytes.

**Parameters**:

- `pemAddress` (`I64`)
- `Address of PEM-encoded text.`
- `pemLength` (`I64`)
- `Length of PEM text.`

**Returns**: — PemBlock:
Decoded PEM block with DER bytes and label.

**Complexity**:
- Time: `O(n) where n is pemLength`
- Space: `O(n) for base64 content + decoded DER`

## function `pemDecodeString`

Decode PEM from a String.

**Parameters**:

- `pemContent` (`String`)
- `PEM-encoded string.`

**Returns**: — PemBlock:
Decoded PEM block.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(n)`

### Methods

#### `function pemDecodeString( String pemContent ) -> PemBlock`

Decode PEM from a String.

**Parameters**:

- `pemContent` (`String`)
- `PEM-encoded string.`

**Returns**: — PemBlock:
Decoded PEM block.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(n)`

## function `findBeginMarker`

Find the offset of "-----BEGIN " in the data.

**Parameters**:

- `dataAddress` (`I64`)
- `Data address.`
- `dataLength` (`I64`)
- `Data length.`

**Returns**: — I64:
Offset of marker, or -1 if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function findBeginMarker( I64 dataAddress, I64 dataLength ) -> I64`

Find the offset of "-----BEGIN " in the data.

**Parameters**:

- `dataAddress` (`I64`)
- `Data address.`
- `dataLength` (`I64`)
- `Data length.`

**Returns**: — I64:
Offset of marker, or -1 if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `findEndMarker`

Find the offset of "-----END " in the data.

**Parameters**:

- `dataAddress` (`I64`)
- `Data address.`
- `dataLength` (`I64`)
- `Data length.`
- `startOffset` (`I64`)
- `Start searching from this offset.`

**Returns**: — I64:
Offset of marker, or -1 if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function findEndMarker( I64 dataAddress, I64 dataLength, I64 startOffset ) -> I64`

Find the offset of "-----END " in the data.

**Parameters**:

- `dataAddress` (`I64`)
- `Data address.`
- `dataLength` (`I64`)
- `Data length.`
- `startOffset` (`I64`)
- `Start searching from this offset.`

**Returns**: — I64:
Offset of marker, or -1 if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

