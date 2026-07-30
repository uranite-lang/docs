# uranite.requests.url

## Table of Contents

- [Imports](#imports)
- [struct `ParsedURL`](#struct-parsedurl)
  - [`ParsedURL()`](#ParsedURL)
- [function `parseIPv4`](#function-parseipv4)
  - [`parseIPv4()`](#parseIPv4)
- [function `parseURL`](#function-parseurl)
  - [`parseURL()`](#parseURL)

## Imports

- `uranite.convert.convert`
  - `stringToInt`
- `uranite.functions.builtin`
  - `charAt`
  - `indexOf`
  - `isDigit`
  - `length`
  - `startsWith`
  - `substring`
- `uranite.net.address`
  - `resolveHostname`
- `uranite.requests.errors`
  - `InvalidURLError`

## struct `ParsedURL`

Holds the parsed components of an HTTP URL: scheme, host, port, path, and the host converted to a packed 32-bit IPv4 address.

### Fields

| Name | Type | Access |
|------|------|--------|
| `scheme` | `String` | public |
| `host` | `String` | public |
| `port` | `I64` | public |
| `path` | `String` | public |
| `ipAddress` | `I64` | public |

### Methods

#### `function ParsedURL( self, String scheme, String host, I64 port, String path, I64 ipAddress ) -> Void`

Construct a ParsedURL with all components.

**Parameters**:

- `scheme` (`String`)
- `The URL scheme.`
- `host` (`String`)
- `The host as a dotted-quad string.`
- `port` (`I64`)
- `The TCP port number.`
- `path` (`String`)
- `The request path.`
- `ipAddress` (`I64`)
- `The host as a packed 32-bit IPv4 address.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `parseIPv4`

Convert a dotted-quad IPv4 address string (e.g., "192.168.1.1") to a packed 32-bit integer in network byte order.

Parses four decimal octets separated by dots and packs them as: (octet0 << 24) | (octet1 << 16) | (octet2 << 8) | octet3.

**Parameters**:

- `hostString` (`String`)
- `The dotted-quad IPv4 address string.`

**Returns**: `I64` — The packed 32-bit IPv4 address.

**Raises**:

- `InvalidURLError` → `RequestError` → `Error` — If the string is not a valid IPv4 address.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

### Methods

#### `function parseIPv4( String hostString ) -> I64`

Convert a dotted-quad IPv4 address string (e.g., "192.168.1.1") to a packed 32-bit integer in network byte order.

Parses four decimal octets separated by dots and packs them as: (octet0 << 24) | (octet1 << 16) | (octet2 << 8) | octet3.

**Parameters**:

- `hostString` (`String`)
- `The dotted-quad IPv4 address string.`

**Returns**: `I64` — The packed 32-bit IPv4 address.

**Raises**:

- `InvalidURLError` → `RequestError` → `Error` — If the string is not a valid IPv4 address.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

## function `parseURL`

Parse an HTTP URL string into its component parts.

Accepts URLs in the form "http://host:port/path" where the port and path are optional. Defaults to port 80 for http and "/" for path. Only "http" scheme is supported (no TLS).

**Parameters**:

- `url` (`String`)
- `The URL string to parse` (`e.g., "http://127.0.0.1:8080/api"`)

**Returns**: `ParsedURL` — The parsed URL components.

**Raises**:

- `InvalidURLError` → `RequestError` → `Error` — If the URL is malformed or uses an unsupported scheme.

**Complexity**:
- Time: `O(n) where n is URL length`
- Space: `O(1)`

### Methods

#### `function parseURL( String url ) -> ParsedURL`

Parse an HTTP URL string into its component parts.

Accepts URLs in the form "http://host:port/path" where the port and path are optional. Defaults to port 80 for http and "/" for path. Only "http" scheme is supported (no TLS).

**Parameters**:

- `url` (`String`)
- `The URL string to parse` (`e.g., "http://127.0.0.1:8080/api"`)

**Returns**: `ParsedURL` — The parsed URL components.

**Raises**:

- `InvalidURLError` → `RequestError` → `Error` — If the URL is malformed or uses an unsupported scheme.

**Complexity**:
- Time: `O(n) where n is URL length`
- Space: `O(1)`

