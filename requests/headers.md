# uranite.requests.headers

## Table of Contents

- [Imports](#imports)
- [class `Headers`](#class-headers)
  - [`Headers()`](#Headers)
  - [`set()`](#set)
  - [`get()`](#get)
  - [`has()`](#has)
  - [`remove()`](#remove)
  - [`toString()`](#toString)
  - [`destroy()`](#destroy)
- [function `parseHeaders`](#function-parseheaders)
  - [`parseHeaders()`](#parseHeaders)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.collection.hash-map`
  - `HashMap`
- `uranite.functions.builtin`
  - `indexOf`
  - `length`
  - `substring`
  - `toLower`
  - `trim`
- `uranite.functions.strings`
  - `split`

## class `Headers`

Case-insensitive HTTP header collection. All header names are stored and looked up in lowercase to conform to HTTP/1.1 semantics where header field names are case-insensitive (RFC 7230 Section 3.2).

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `HashMap<String, String>` | private |

### Methods

#### `function Headers( self ) -> Void`

Create an empty Headers collection.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function set( self, String name, String value ) -> Headers`

Set a header value, replacing any existing value for the same name.

**Parameters**:

- `name` (`String`)
- `The header name` (`case-insensitive`)
- `value` (`String`)
- `The header value.`

**Returns**: `Headers` — This instance for method chaining.

#### `function get( self, String name ) -> String`

Retrieve the value of a header by name.

**Parameters**:

- `name` (`String`)
- `The header name` (`case-insensitive`)

**Returns**: `String` — The header value, or empty string if not present.

#### `function has( self, String name ) -> Boolean`

Check whether a header exists by name.

**Parameters**:

- `name` (`String`)
- `The header name` (`case-insensitive`)

**Returns**: `Boolean` — True if the header is present.

#### `function remove( self, String name ) -> Void`

Remove a header by name.

**Parameters**:

- `name` (`String`)
- `The header name` (`case-insensitive`)

#### `function toString( self ) -> String`

Serialize all headers into HTTP wire format (Name: Value\\r\\n pairs).

**Returns**: — String:
The serialized headers.

**Complexity**:
- Time: `O(n) where n is number of headers`
- Space: `O(n)`

#### `function destroy( self ) -> Void`

Release resources held by the internal map.

## function `parseHeaders`

Parse a raw HTTP header block into a Headers object.

Splits the raw string on CRLF boundaries and extracts name-value pairs separated by the first colon in each line.

**Parameters**:

- `rawHeaders` (`String`)
- `The raw headers string from an HTTP response.`

**Returns**: — Headers:
A populated Headers collection.

**Complexity**:
- Time: `O(n) where n is raw header string length`
- Space: `O(h) where h is number of distinct headers`

### Methods

#### `function parseHeaders( String rawHeaders ) -> Headers`

Parse a raw HTTP header block into a Headers object.

Splits the raw string on CRLF boundaries and extracts name-value pairs separated by the first colon in each line.

**Parameters**:

- `rawHeaders` (`String`)
- `The raw headers string from an HTTP response.`

**Returns**: — Headers:
A populated Headers collection.

**Complexity**:
- Time: `O(n) where n is raw header string length`
- Space: `O(h) where h is number of distinct headers`

