# uranite.convert.convert

## Table of Contents

- [Imports](#imports)
- [class `ConversionError`](#class-conversionerror)
  - [`ConversionError()`](#ConversionError)
- [function `intToString`](#function-inttostring)
  - [`intToString()`](#intToString)
- [function `stringToInt`](#function-stringtoint)
  - [`stringToInt()`](#stringToInt)
- [function `floatToString`](#function-floattostring)
  - [`floatToString()`](#floatToString)
- [function `stringToFloat`](#function-stringtofloat)
  - [`stringToFloat()`](#stringToFloat)
- [function `boolToString`](#function-booltostring)
  - [`boolToString()`](#boolToString)
- [function `hexToInt`](#function-hextoint)
  - [`hexToInt()`](#hexToInt)
- [function `intToHex`](#function-inttohex)
  - [`intToHex()`](#intToHex)
- [function `intToBin`](#function-inttobin)
  - [`intToBin()`](#intToBin)
- [function `tryParseInt`](#function-tryparseint)
  - [`tryParseInt()`](#tryParseInt)
- [function `tryParseFloat`](#function-tryparsefloat)
  - [`tryParseFloat()`](#tryParseFloat)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`

## class `ConversionError`

**Extends**: `Error`

Raised when a type conversion fails, such as attempting to parse an invalid string as a number or encountering unexpected characters during hex or binary conversion.

### Methods

#### `function ConversionError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new ConversionError.

**Parameters**:

- `message` (`String`)
- `A description of what conversion failed and why.`
- `code` (`I64`)
- `A numeric error code.`
- `cause` (`?Error`)
- `An optional underlying error, or None.`

## function `intToString`

Convert an I64 integer value to its decimal string representation. Handles negative values by prepending a minus sign. Allocates a new null-terminated string buffer via mmap.

**Parameters**:

- `value` (`I64`)
- `The integer value to convert.`

**Returns**: — The decimal string representation of the value.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function intToString( I64 value ) -> String`

Convert an I64 integer value to its decimal string representation. Handles negative values by prepending a minus sign. Allocates a new null-terminated string buffer via mmap.

**Parameters**:

- `value` (`I64`)
- `The integer value to convert.`

**Returns**: — The decimal string representation of the value.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `stringToInt`

Parse a decimal integer string into an I64 value. Accepts an optional leading '+' or '-' sign followed by one or more digits.

**Parameters**:

- `source` (`String`)
- `The string to parse.`

**Returns**: — The parsed integer value.

**Raises**:

- `ConversionError` → `Error` — If the string is empty, contains only a sign character, or includes non-digit characters.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function stringToInt( String source ) -> I64`

Parse a decimal integer string into an I64 value. Accepts an optional leading '+' or '-' sign followed by one or more digits.

**Parameters**:

- `source` (`String`)
- `The string to parse.`

**Returns**: — The parsed integer value.

**Raises**:

- `ConversionError` → `Error` — If the string is empty, contains only a sign character, or includes non-digit characters.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `floatToString`

Convert an F64 floating-point value to its string representation with the specified number of decimal digits. Uses half-up rounding for the fractional part.

**Parameters**:

- `value` (`F64`)
- `The floating-point value to convert.`
- `precision` (`I64`)
- `The number of digits after the decimal point. Values less`
- `than zero default to 6.`

**Returns**: — The formatted string representation with exactly the specified
number of decimal digits.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function floatToString( F64 value, I64 precision ) -> String`

Convert an F64 floating-point value to its string representation with the specified number of decimal digits. Uses half-up rounding for the fractional part.

**Parameters**:

- `value` (`F64`)
- `The floating-point value to convert.`
- `precision` (`I64`)
- `The number of digits after the decimal point. Values less`
- `than zero default to 6.`

**Returns**: — The formatted string representation with exactly the specified
number of decimal digits.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `stringToFloat`

Parse a floating-point string into an F64 value. Accepts an optional leading '+' or '-' sign, followed by digits with an optional decimal point separating the integer and fractional parts.

**Parameters**:

- `source` (`String`)
- `The string to parse.`

**Returns**: — The parsed floating-point value.

**Raises**:

- `ConversionError` → `Error` — If the string is empty or contains invalid characters.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function stringToFloat( String source ) -> F64`

Parse a floating-point string into an F64 value. Accepts an optional leading '+' or '-' sign, followed by digits with an optional decimal point separating the integer and fractional parts.

**Parameters**:

- `source` (`String`)
- `The string to parse.`

**Returns**: — The parsed floating-point value.

**Raises**:

- `ConversionError` → `Error` — If the string is empty or contains invalid characters.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `boolToString`

Convert a boolean value to its string representation.

**Parameters**:

- `value` (`Boolean`)
- `The boolean to convert.`

**Returns**: — "true" if the value is True, "false" otherwise.

### Methods

#### `function boolToString( Boolean value ) -> String`

Convert a boolean value to its string representation.

**Parameters**:

- `value` (`Boolean`)
- `The boolean to convert.`

**Returns**: — "true" if the value is True, "false" otherwise.

## function `hexToInt`

Parse a hexadecimal string into an I64 value. Accepts an optional "0x" or "0X" prefix. Digits may be uppercase (A-F) or lowercase (a-f).

**Parameters**:

- `source` (`String`)
- `The hexadecimal string to parse.`

**Returns**: — The parsed integer value.

**Raises**:

- `ConversionError` → `Error` — If the string contains invalid hex characters.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function hexToInt( String source ) -> I64`

Parse a hexadecimal string into an I64 value. Accepts an optional "0x" or "0X" prefix. Digits may be uppercase (A-F) or lowercase (a-f).

**Parameters**:

- `source` (`String`)
- `The hexadecimal string to parse.`

**Returns**: — The parsed integer value.

**Raises**:

- `ConversionError` → `Error` — If the string contains invalid hex characters.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `intToHex`

Convert an I64 integer value to its hexadecimal string representation prefixed with "0x". Uses lowercase hex digits (a-f). Negative values are preceded by a minus sign.

**Parameters**:

- `value` (`I64`)
- `The integer value to convert.`

**Returns**: — The hexadecimal string representation (e.g. "0x1a3f").

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function intToHex( I64 value ) -> String`

Convert an I64 integer value to its hexadecimal string representation prefixed with "0x". Uses lowercase hex digits (a-f). Negative values are preceded by a minus sign.

**Parameters**:

- `value` (`I64`)
- `The integer value to convert.`

**Returns**: — The hexadecimal string representation (e.g. "0x1a3f").

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `intToBin`

Convert an I64 integer value to its binary string representation prefixed with "0b". Negative values are preceded by a minus sign.

**Parameters**:

- `value` (`I64`)
- `The integer value to convert.`

**Returns**: — The binary string representation (e.g. "0b1010").

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function intToBin( I64 value ) -> String`

Convert an I64 integer value to its binary string representation prefixed with "0b". Negative values are preceded by a minus sign.

**Parameters**:

- `value` (`I64`)
- `The integer value to convert.`

**Returns**: — The binary string representation (e.g. "0b1010").

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `tryParseInt`

Parse a decimal integer string into an I64, returning None on failure instead of raising. Accepts optional leading sign followed by digits.

**Parameters**:

- `source` (`String`)
- `The string to parse.`

**Returns**: — ?I64:
The parsed integer, or None if the string is not a valid integer.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function tryParseInt( String source ) -> ?I64`

Parse a decimal integer string into an I64, returning None on failure instead of raising. Accepts optional leading sign followed by digits.

**Parameters**:

- `source` (`String`)
- `The string to parse.`

**Returns**: — ?I64:
The parsed integer, or None if the string is not a valid integer.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `tryParseFloat`

Parse a decimal floating-point string into an F64, returning None on failure instead of raising. Accepts optional sign, digits, optional decimal point with fractional digits.

**Parameters**:

- `source` (`String`)
- `The string to parse.`

**Returns**: — ?F64:
The parsed float, or None if the string is not a valid float.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function tryParseFloat( String source ) -> ?F64`

Parse a decimal floating-point string into an F64, returning None on failure instead of raising. Accepts optional sign, digits, optional decimal point with fractional digits.

**Parameters**:

- `source` (`String`)
- `The string to parse.`

**Returns**: — ?F64:
The parsed float, or None if the string is not a valid float.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

