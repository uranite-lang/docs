# uranite.net.http.parser

## Table of Contents

- [Imports](#imports)
- [function `findCRLF`](#function-findcrlf)
  - [`findCRLF()`](#findCRLF)
- [function `parseStatusCode`](#function-parsestatuscode)
  - [`parseStatusCode()`](#parseStatusCode)
- [function `extractStatusText`](#function-extractstatustext)
  - [`extractStatusText()`](#extractStatusText)
- [function `parseContentLength`](#function-parsecontentlength)
  - [`parseContentLength()`](#parseContentLength)
- [function `parseResponse`](#function-parseresponse)
  - [`parseResponse()`](#parseResponse)

## Imports

- `uranite.functions.builtin`
  - `ptrToString`
  - `substring`
  - `trim`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
- `uranite.net.http.errors`
  - `HttpParseError`
- `uranite.net.http.response`
  - `HttpResponse`

## function `findCRLF`

Find the next CRLF (\\r\\n) sequence in a raw byte buffer.

**Parameters**:

- `dataPointer` (`I64`)
- `Raw pointer to the byte buffer to search.`
- `startOffset` (`I64`)
- `Byte offset to begin searching from.`
- `dataLength` (`I64`)
- `Total length of the buffer in bytes.`

**Returns**: — The byte offset of the CR byte, or -1 if no CRLF found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function findCRLF( I64 dataPointer, I64 startOffset, I64 dataLength ) -> I64`

Find the next CRLF (\\r\\n) sequence in a raw byte buffer.

**Parameters**:

- `dataPointer` (`I64`)
- `Raw pointer to the byte buffer to search.`
- `startOffset` (`I64`)
- `Byte offset to begin searching from.`
- `dataLength` (`I64`)
- `Total length of the buffer in bytes.`

**Returns**: — The byte offset of the CR byte, or -1 if no CRLF found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `parseStatusCode`

Extract the numeric status code from an HTTP status line.

**Parameters**:

- `raw` (`String`)
- `The full status line` (`e.g. "HTTP/1.1 200 OK"`)

**Returns**: — The parsed status code as an integer.

**Raises**:

- `HttpParseError` → `HttpError` → `Error` — If the status line has no space or contains non-digit characters in the status code position.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function parseStatusCode( String raw ) -> I64`

Extract the numeric status code from an HTTP status line.

**Parameters**:

- `raw` (`String`)
- `The full status line` (`e.g. "HTTP/1.1 200 OK"`)

**Returns**: — The parsed status code as an integer.

**Raises**:

- `HttpParseError` → `HttpError` → `Error` — If the status line has no space or contains non-digit characters in the status code position.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `extractStatusText`

Extract the reason phrase from an HTTP status line.

**Parameters**:

- `raw` (`String`)
- `The full status line` (`e.g. "HTTP/1.1 200 OK"`)

**Returns**: — The trimmed reason phrase (e.g. "OK"), or empty string if none.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function extractStatusText( String raw ) -> String`

Extract the reason phrase from an HTTP status line.

**Parameters**:

- `raw` (`String`)
- `The full status line` (`e.g. "HTTP/1.1 200 OK"`)

**Returns**: — The trimmed reason phrase (e.g. "OK"), or empty string if none.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `parseContentLength`

Search for and parse the Content-Length header value from a raw header block. Case-insensitive matching.

**Parameters**:

- `headers` (`String`)
- `The raw header block as a single string.`

**Returns**: — The parsed content length, or -1 if the header is not present.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function parseContentLength( String headers ) -> I64`

Search for and parse the Content-Length header value from a raw header block. Case-insensitive matching.

**Parameters**:

- `headers` (`String`)
- `The raw header block as a single string.`

**Returns**: — The parsed content length, or -1 if the header is not present.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `parseResponse`

Parse a raw HTTP response string into an HttpResponse object. Splits the response at the CRLFCRLF boundary into headers and body, then extracts the status code, reason phrase, and Content-Length.

**Parameters**:

- `rawData` (`String`)
- `The complete raw HTTP response.`

**Returns**: — A fully populated HttpResponse.

**Raises**:

- `HttpParseError` → `HttpError` → `Error` — If the response lacks a header terminator or status line.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function parseResponse( String rawData ) -> HttpResponse`

Parse a raw HTTP response string into an HttpResponse object. Splits the response at the CRLFCRLF boundary into headers and body, then extracts the status code, reason phrase, and Content-Length.

**Parameters**:

- `rawData` (`String`)
- `The complete raw HTTP response.`

**Returns**: — A fully populated HttpResponse.

**Raises**:

- `HttpParseError` → `HttpError` → `Error` — If the response lacks a header terminator or status line.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

