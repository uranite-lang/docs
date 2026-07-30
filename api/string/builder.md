# uranite.string.builder

## Table of Contents

- [Imports](#imports)
- [const `INITIAL_CAPACITY`](#const-initial-capacity)
- [class `StringBuilder`](#class-stringbuilder)
  - [`StringBuilder()`](#StringBuilder)
  - [`grow()`](#grow)
  - [`append()`](#append)
  - [`appendChar()`](#appendChar)
  - [`appendI64()`](#appendI64)
  - [`appendF64()`](#appendF64)
  - [`appendBool()`](#appendBool)
  - [`appendLine()`](#appendLine)
  - [`build()`](#build)
  - [`clear()`](#clear)
  - [`length()`](#length)
  - [`capacity()`](#capacity)
  - [`isEmpty()`](#isEmpty)
  - [`charAtIndex()`](#charAtIndex)
  - [`toString()`](#toString)
  - [`appendRepeated()`](#appendRepeated)
  - [`reverse()`](#reverse)
  - [`substring()`](#substring)
  - [`indexOf()`](#indexOf)
  - [`insert()`](#insert)
  - [`deleteRange()`](#deleteRange)
  - [`replaceRange()`](#replaceRange)
  - [`destroy()`](#destroy)

## Imports

- `uranite.convert.convert`
  - `floatToString`
  - `intToString`
- `uranite.errors.lookup`
  - `IndexError`
- `uranite.errors.value`
  - `ValueError`
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
  - `realloc`

## const `INITIAL_CAPACITY`

The default initial buffer capacity in bytes. 

## class `StringBuilder`

A mutable byte buffer for efficient incremental string construction. Avoids the O(n^2) cost of repeated string concatenation by maintaining a dynamically growing internal buffer. The buffer doubles in capacity when more space is needed. Uses mmap-based allocation with zero C runtime dependency. Call build() to produce the final immutable String, and destroy() to release the underlying memory when done.

### Fields

| Name | Type | Access |
|------|------|--------|
| `buffer` | `I64` | public |
| `len` | `I64` | public |
| `cap` | `I64` | public |

### Methods

#### `function StringBuilder( self ) -> Void`

Create a new StringBuilder with the default initial capacity of 64 bytes.

#### `function grow( self, I64 needed ) -> Void`

Ensure that at least the given number of additional bytes can be written without reallocation. If the current capacity is insufficient, the buffer is reallocated with doubled capacity until it is large enough.

**Parameters**:

- `needed` (`I64`)
- `The number of additional bytes required.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function append( self, String content ) -> StringBuilder`

Append a string to the buffer. Returns self for method chaining.

**Parameters**:

- `content` (`String`)
- `The string to append.`

**Returns**: — This StringBuilder instance for chaining.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function appendChar( self, I64 codePoint ) -> StringBuilder`

Append a single byte to the buffer. Returns self for method chaining.

**Parameters**:

- `codePoint` (`I64`)
- `The byte value to append` (`0-255`)

**Returns**: — This StringBuilder instance for chaining.

#### `function appendI64( self, I64 value ) -> StringBuilder`

Append the decimal string representation of an I64 value to the buffer. Returns self for method chaining.

**Parameters**:

- `value` (`I64`)
- `The integer value to convert and append.`

**Returns**: — This StringBuilder instance for chaining.

#### `function appendF64( self, F64 value ) -> StringBuilder`

Append the string representation of an F64 value to the buffer with 6 digits of decimal precision. Returns self for method chaining.

**Parameters**:

- `value` (`F64`)
- `The floating-point value to convert and append.`

**Returns**: — This StringBuilder instance for chaining.

#### `function appendBool( self, Boolean value ) -> StringBuilder`

Append "true" or "false" to the buffer based on the given boolean value. Returns self for method chaining.

**Parameters**:

- `value` (`Boolean`)
- `The boolean value to convert and append.`

**Returns**: — This StringBuilder instance for chaining.

#### `function appendLine( self, String content ) -> StringBuilder`

Append a string followed by a newline character (byte 10) to the buffer. Returns self for method chaining.

**Parameters**:

- `content` (`String`)
- `The string to append before the newline.`

**Returns**: — This StringBuilder instance for chaining.

#### `function build( self ) -> String`

Produce the final immutable String from the buffer contents. Allocates a new null-terminated buffer, copies all bytes, and returns the result as a String. The StringBuilder remains usable after this call.

**Returns**: — A new String containing all bytes appended so far.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function clear( self ) -> Void`

Reset the buffer length to zero without deallocating memory. The existing capacity is retained for future appends.

#### `function length( self ) -> I64`

Return the current number of bytes in the buffer.

**Returns**: — The number of bytes written so far.

#### `function capacity( self ) -> I64`

Return the current allocated capacity of the buffer.

**Returns**: — The total number of bytes the buffer can hold before
needing to grow.

#### `function isEmpty( self ) -> Boolean`

Check whether the buffer is empty.

**Returns**: — True if no bytes have been written, False otherwise.

#### `function charAtIndex( self, I64 index ) -> I64`

Return the byte value at the given index in the buffer.

**Parameters**:

- `index` (`I64`)
- `The zero-based byte index to read.`

**Returns**: — The byte value at the index, or -1 if the index is out
of bounds.

#### `function toString( self ) -> String`

Return the accumulated buffer contents as an immutable String.

Alias for build() to match the standard toString convention.

**Returns**: — String:
The final string.

**Complexity**:
- Time: `O(n) where n is the buffer length`
- Space: `O(n)`

#### `function appendRepeated( self, String content, I64 repeatCount ) -> StringBuilder`

Append the content string the specified number of times.

**Parameters**:

- `content` (`String`)
- `The string to repeat.`
- `repeatCount` (`I64`)
- `How many times to append the content.`

**Returns**: `StringBuilder` — This builder for method chaining.

**Raises**:

- `ValueError` → `Error` — If repeatCount is negative.

**Complexity**:
- Time: `O(repeatCount * len(content))`
- Space: `O(repeatCount * len(content))`

#### `function reverse( self ) -> StringBuilder`

Reverse the buffer contents in place.

**Returns**: — StringBuilder:
This builder for method chaining.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function substring( self, I64 startIndex, I64 endIndex ) -> String`

Extract a substring from the buffer in the range [startIndex, endIndex).

**Parameters**:

- `startIndex` (`I64`)
- `The inclusive start byte index.`
- `endIndex` (`I64`)
- `The exclusive end byte index.`

**Returns**: `String` — The extracted substring.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If either index is out of bounds or startIndex > endIndex.

**Complexity**:
- Time: `O(endIndex - startIndex)`
- Space: `O(endIndex - startIndex)`

#### `function indexOf( self, String needle ) -> I64`

Find the first occurrence of the needle string in the buffer.

**Parameters**:

- `needle` (`String`)
- `The string to search for.`

**Returns**: — I64:
The byte index of the first match, or -1 if not found.

**Complexity**:
- Time: `O(n * m) where n is the buffer length and m is the needle length`
- Space: `O(1)`

#### `function insert( self, I64 position, String content ) -> StringBuilder`

Insert a string at the specified byte position, shifting existing content to the right.

**Parameters**:

- `position` (`I64`)
- `The byte index at which to insert.`
- `content` (`String`)
- `The string to insert.`

**Returns**: `StringBuilder` — This builder for method chaining.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If position is out of bounds.

**Complexity**:
- Time: `O(n) where n is the buffer length`

#### `function deleteRange( self, I64 startIndex, I64 endIndex ) -> StringBuilder`

Remove bytes in the range [startIndex, endIndex) from the buffer, shifting remaining content to the left.

**Parameters**:

- `startIndex` (`I64`)
- `The inclusive start byte index.`
- `endIndex` (`I64`)
- `The exclusive end byte index.`

**Returns**: `StringBuilder` — This builder for method chaining.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If either index is out of bounds or startIndex > endIndex.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function replaceRange( self, I64 startIndex, I64 endIndex, String replacement ) -> StringBuilder`

Replace bytes in [startIndex, endIndex) with the replacement string.

**Parameters**:

- `startIndex` (`I64`)
- `The inclusive start byte index.`
- `endIndex` (`I64`)
- `The exclusive end byte index.`
- `replacement` (`String`)
- `The string to insert in place of the removed range.`

**Returns**: `StringBuilder` — This builder for method chaining.

**Raises**:

- `IndexError` → `LookupError` → `Error` — If either index is out of bounds or startIndex > endIndex.

**Complexity**:
- Time: `O(n) where n is the buffer length`

#### `function destroy( self ) -> Void`

Release the underlying buffer memory via munmap and reset all internal state to zero. The StringBuilder must not be used after calling this method.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

