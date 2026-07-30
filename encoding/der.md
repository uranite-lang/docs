# uranite.encoding.der

## Table of Contents

- [Imports](#imports)
- [const `DER_TAG_INTEGER`](#const-der-tag-integer)
- [const `DER_TAG_BIT_STRING`](#const-der-tag-bit-string)
- [const `DER_TAG_OCTET_STRING`](#const-der-tag-octet-string)
- [const `DER_TAG_NULL`](#const-der-tag-null)
- [const `DER_TAG_OID`](#const-der-tag-oid)
- [const `DER_TAG_SEQUENCE`](#const-der-tag-sequence)
- [const `DER_TAG_SET`](#const-der-tag-set)
- [const `DER_TAG_CONTEXT_0`](#const-der-tag-context-0)
- [const `DER_TAG_CONTEXT_1`](#const-der-tag-context-1)
- [const `DER_TAG_CONTEXT_3`](#const-der-tag-context-3)
- [struct `DerElement`](#struct-derelement)
  - [`DerElement()`](#DerElement)
- [function `derParseElement`](#function-derparseelement)
  - [`derParseElement()`](#derParseElement)
- [function `derParseSequence`](#function-derparsesequence)
  - [`derParseSequence()`](#derParseSequence)
- [function `derParseInteger`](#function-derparseinteger)
  - [`derParseInteger()`](#derParseInteger)
- [function `derIntegerToI64`](#function-derintegertoi64)
  - [`derIntegerToI64()`](#derIntegerToI64)
- [function `derGetIntegerBytes`](#function-dergetintegerbytes)
  - [`derGetIntegerBytes()`](#derGetIntegerBytes)
- [function `derParseBitString`](#function-derparsebitstring)
  - [`derParseBitString()`](#derParseBitString)
- [function `derIterateSequence`](#function-deriteratesequence)
  - [`derIterateSequence()`](#derIterateSequence)
- [function `derCountSequenceElements`](#function-dercountsequenceelements)
  - [`derCountSequenceElements()`](#derCountSequenceElements)

## Imports

- `uranite.io.syscall`
  - `readByteAt`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`

## const `DER_TAG_INTEGER`

## const `DER_TAG_BIT_STRING`

## const `DER_TAG_OCTET_STRING`

## const `DER_TAG_NULL`

## const `DER_TAG_OID`

## const `DER_TAG_SEQUENCE`

## const `DER_TAG_SET`

## const `DER_TAG_CONTEXT_0`

## const `DER_TAG_CONTEXT_1`

## const `DER_TAG_CONTEXT_3`

## struct `DerElement`

A parsed DER TLV element. Holds the tag, content address and length, and total element size (including tag and length bytes).

### Fields

| Name | Type | Access |
|------|------|--------|
| `tag` | `I64` | public |
| `contentAddress` | `I64` | public |
| `contentLength` | `I64` | public |
| `headerSize` | `I64` | public |
| `totalSize` | `I64` | public |

### Methods

#### `function DerElement( self, I64 tag, I64 contentAddress, I64 contentLength, I64 headerSize ) -> Void`

**Parameters**:

- `tag` (`I64`)
- `ASN.1 tag byte.`
- `contentAddress` (`I64`)
- `Address of content bytes.`
- `contentLength` (`I64`)
- `Length of content.`
- `headerSize` (`I64`)
- `Size of tag + length encoding.`

## function `derParseElement`

Parse a single DER TLV element at the given offset.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address of DER data.`
- `dataOffset` (`I64`)
- `Offset to start parsing.`
- `dataLength` (`I64`)
- `Total available data length.`

**Returns**: — DerElement:
Parsed element with tag, content address/length.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function derParseElement( I64 dataAddress, I64 dataOffset, I64 dataLength ) -> DerElement`

Parse a single DER TLV element at the given offset.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address of DER data.`
- `dataOffset` (`I64`)
- `Offset to start parsing.`
- `dataLength` (`I64`)
- `Total available data length.`

**Returns**: — DerElement:
Parsed element with tag, content address/length.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `derParseSequence`

Parse a DER SEQUENCE element. Verifies the tag is 0x30.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address.`
- `dataOffset` (`I64`)
- `Offset to parse at.`
- `dataLength` (`I64`)
- `Available data.`

**Returns**: — DerElement:
Parsed SEQUENCE element.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function derParseSequence( I64 dataAddress, I64 dataOffset, I64 dataLength ) -> DerElement`

Parse a DER SEQUENCE element. Verifies the tag is 0x30.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address.`
- `dataOffset` (`I64`)
- `Offset to parse at.`
- `dataLength` (`I64`)
- `Available data.`

**Returns**: — DerElement:
Parsed SEQUENCE element.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `derParseInteger`

Parse a DER INTEGER element.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address.`
- `dataOffset` (`I64`)
- `Offset to parse at.`
- `dataLength` (`I64`)
- `Available data.`

**Returns**: — DerElement:
Parsed INTEGER element.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function derParseInteger( I64 dataAddress, I64 dataOffset, I64 dataLength ) -> DerElement`

Parse a DER INTEGER element.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address.`
- `dataOffset` (`I64`)
- `Offset to parse at.`
- `dataLength` (`I64`)
- `Available data.`

**Returns**: — DerElement:
Parsed INTEGER element.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `derIntegerToI64`

Convert a small DER INTEGER to an I64 value.

**Parameters**:

- `element` (`DerElement`)
- `Parsed INTEGER element.`

**Returns**: — I64:
Integer value (truncated to 64 bits).

**Complexity**:
- Time: `O(n) where n is content length`
- Space: `O(1)`

### Methods

#### `function derIntegerToI64( DerElement element ) -> I64`

Convert a small DER INTEGER to an I64 value.

**Parameters**:

- `element` (`DerElement`)
- `Parsed INTEGER element.`

**Returns**: — I64:
Integer value (truncated to 64 bits).

**Complexity**:
- Time: `O(n) where n is content length`
- Space: `O(1)`

## function `derGetIntegerBytes`

Get the address of integer content bytes, skipping any leading zero padding byte (used for unsigned representation).

**Parameters**:

- `element` (`DerElement`)
- `Parsed INTEGER element.`

**Returns**: — I64:
Packed result: bits [0..31] = content address offset,
bits [32..63] = effective length.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function derGetIntegerBytes( DerElement element ) -> I64`

Get the address of integer content bytes, skipping any leading zero padding byte (used for unsigned representation).

**Parameters**:

- `element` (`DerElement`)
- `Parsed INTEGER element.`

**Returns**: — I64:
Packed result: bits [0..31] = content address offset,
bits [32..63] = effective length.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `derParseBitString`

Parse a DER BIT STRING element. The first content byte indicates unused bits in the last byte.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address.`
- `dataOffset` (`I64`)
- `Offset to parse at.`
- `dataLength` (`I64`)
- `Available data.`

**Returns**: — DerElement:
Parsed BIT STRING element.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function derParseBitString( I64 dataAddress, I64 dataOffset, I64 dataLength ) -> DerElement`

Parse a DER BIT STRING element. The first content byte indicates unused bits in the last byte.

**Parameters**:

- `dataAddress` (`I64`)
- `Base address.`
- `dataOffset` (`I64`)
- `Offset to parse at.`
- `dataLength` (`I64`)
- `Available data.`

**Returns**: — DerElement:
Parsed BIT STRING element.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `derIterateSequence`

Get the Nth child element from a SEQUENCE.

**Parameters**:

- `sequence` (`DerElement`)
- `Parent SEQUENCE element.`
- `elementIndex` (`I64`)
- `Zero-based child index.`

**Returns**: — DerElement:
Child element at the given index.

**Complexity**:
- Time: `O(n) where n is elementIndex`
- Space: `O(1)`

### Methods

#### `function derIterateSequence( DerElement sequence, I64 elementIndex ) -> DerElement`

Get the Nth child element from a SEQUENCE.

**Parameters**:

- `sequence` (`DerElement`)
- `Parent SEQUENCE element.`
- `elementIndex` (`I64`)
- `Zero-based child index.`

**Returns**: — DerElement:
Child element at the given index.

**Complexity**:
- Time: `O(n) where n is elementIndex`
- Space: `O(1)`

## function `derCountSequenceElements`

Count children in a SEQUENCE.

**Parameters**:

- `sequence` (`DerElement`)
- `Parent SEQUENCE element.`

**Returns**: — I64:
Number of child elements.

**Complexity**:
- Time: `O(n) where n is number of children`
- Space: `O(1)`

### Methods

#### `function derCountSequenceElements( DerElement sequence ) -> I64`

Count children in a SEQUENCE.

**Parameters**:

- `sequence` (`DerElement`)
- `Parent SEQUENCE element.`

**Returns**: — I64:
Number of child elements.

**Complexity**:
- Time: `O(n) where n is number of children`
- Space: `O(1)`

