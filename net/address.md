# uranite.net.address

## Table of Contents

- [Imports](#imports)
- [struct `HostAddress`](#struct-hostaddress)
  - [`HostAddress()`](#HostAddress)
- [function `parseIpv4`](#function-parseipv4)
  - [`parseIpv4()`](#parseIpv4)
- [function `resolveHostname`](#function-resolvehostname)
  - [`resolveHostname()`](#resolveHostname)
- [function `matchHostsLine`](#function-matchhostsline)
- [function `parseIpv4Range`](#function-parseipv4range)
- [function `parseHostPort`](#function-parsehostport)
  - [`parseHostPort()`](#parseHostPort)

## Imports

- `uranite.io.syscall`
  - `O_RDONLY`
  - `SYS_CLOSE`
  - `SYS_OPEN`
  - `SYS_READ`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`
- `uranite.net.dns`
  - `DnsResolver`
- `uranite.net.errors`
  - `SocketError`
- `uranite.net.socket`
  - `ipToInt`
- `uranite.os.syscall.invoke`
  - `syscall1`
  - `syscall3`
- `uranite.os.syscall.result`
  - `SyscallResult`

## struct `HostAddress`

Resolved network endpoint containing the original host string, parsed port number, and the IPv4 address as a packed 32-bit integer ready for socket operations.

### Fields

| Name | Type | Access |
|------|------|--------|
| `host` | `String` | public |
| `port` | `I64` | public |
| `ipAddress` | `I64` | public |

### Methods

#### `function HostAddress( self, String host, I64 port, I64 ipAddress ) -> Void`

**Parameters**:

- `host` (`String`)
- `Original host string.`
- `port` (`I64`)
- `Port number.`
- `ipAddress` (`I64`)
- `Packed IPv4 address integer.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `parseIpv4`

Parse a dotted-decimal IPv4 address string into a packed 32-bit integer in network byte order suitable for socket operations.

**Parameters**:

- `ipString` (`String`)
- `IPv4 address in "A.B.C.D" format` (`e.g., "192.168.1.100"`)

**Returns**: — I64:
Packed IPv4 address integer, or 0 if the format is invalid.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

### Methods

#### `function parseIpv4( String ipString ) -> I64`

Parse a dotted-decimal IPv4 address string into a packed 32-bit integer in network byte order suitable for socket operations.

**Parameters**:

- `ipString` (`String`)
- `IPv4 address in "A.B.C.D" format` (`e.g., "192.168.1.100"`)

**Returns**: — I64:
Packed IPv4 address integer, or 0 if the format is invalid.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

## function `resolveHostname`

Resolve a hostname to a packed IPv4 address. First checks /etc/hosts for a local match. If not found, performs a DNS A-record query via UDP to the system nameserver (from /etc/resolv.conf, defaulting to 127.0.0.1).

**Parameters**:

- `hostname` (`String`)
- `Hostname to resolve` (`e.g., "localhost", "www.google.com"`)

**Returns**: — I64:
Packed IPv4 address, or -1 if resolution fails via both
/etc/hosts and DNS.

**Complexity**:
- Time: `O(f * h + n) where f is hosts file size, h is hostname`
- Space: `O(f)`

### Methods

#### `function resolveHostname( String hostname ) -> I64`

Resolve a hostname to a packed IPv4 address. First checks /etc/hosts for a local match. If not found, performs a DNS A-record query via UDP to the system nameserver (from /etc/resolv.conf, defaulting to 127.0.0.1).

**Parameters**:

- `hostname` (`String`)
- `Hostname to resolve` (`e.g., "localhost", "www.google.com"`)

**Returns**: — I64:
Packed IPv4 address, or -1 if resolution fails via both
/etc/hosts and DNS.

**Complexity**:
- Time: `O(f * h + n) where f is hosts file size, h is hostname`
- Space: `O(f)`

## function `matchHostsLine`

Check a single /etc/hosts line for a hostname match. Returns the packed IPv4 address if matched, -1 otherwise. Skips comment lines (starting with #) and IPv6 addresses.

**Parameters**:

- `buffer` (`I64`)
- `Address of the file buffer.`
- `lineStart` (`I64`)
- `Start offset of the line in the buffer.`
- `lineEnd` (`I64`)
- `End offset` (`exclusive`)
- `hostnameAddress` (`I64`)
- `Address of the hostname string to match.`
- `hostnameLength` (`I64`)
- `Length of the hostname string.`

**Returns**: — I64:
Packed IPv4 address on match, -1 otherwise.

**Complexity**:
- Time: `O(lineEnd - lineStart)`
- Space: `O(1)`

### Methods

#### `function matchHostsLine( I64 buffer, I64 lineStart, I64 lineEnd, I64 hostnameAddress, I64 hostnameLength ) -> I64`

Check a single /etc/hosts line for a hostname match. Returns the packed IPv4 address if matched, -1 otherwise. Skips comment lines (starting with #) and IPv6 addresses.

**Parameters**:

- `buffer` (`I64`)
- `Address of the file buffer.`
- `lineStart` (`I64`)
- `Start offset of the line in the buffer.`
- `lineEnd` (`I64`)
- `End offset` (`exclusive`)
- `hostnameAddress` (`I64`)
- `Address of the hostname string to match.`
- `hostnameLength` (`I64`)
- `Length of the hostname string.`

**Returns**: — I64:
Packed IPv4 address on match, -1 otherwise.

**Complexity**:
- Time: `O(lineEnd - lineStart)`
- Space: `O(1)`

## function `parseIpv4Range`

Parse a dotted-decimal IPv4 address from a byte range in a buffer into a packed 32-bit integer.

**Parameters**:

- `buffer` (`I64`)
- `Address of the buffer containing the IP string.`
- `rangeStart` (`I64`)
- `Start offset of the IP string.`
- `rangeEnd` (`I64`)
- `End offset` (`exclusive`)

**Returns**: — I64:
Packed IPv4 address, or -1 if the format is invalid.

**Complexity**:
- Time: `O(rangeEnd - rangeStart)`
- Space: `O(1)`

### Methods

#### `function parseIpv4Range( I64 buffer, I64 rangeStart, I64 rangeEnd ) -> I64`

Parse a dotted-decimal IPv4 address from a byte range in a buffer into a packed 32-bit integer.

**Parameters**:

- `buffer` (`I64`)
- `Address of the buffer containing the IP string.`
- `rangeStart` (`I64`)
- `Start offset of the IP string.`
- `rangeEnd` (`I64`)
- `End offset` (`exclusive`)

**Returns**: — I64:
Packed IPv4 address, or -1 if the format is invalid.

**Complexity**:
- Time: `O(rangeEnd - rangeStart)`
- Space: `O(1)`

## function `parseHostPort`

Parse a "host:port" string into its components and resolve the host to a packed IPv4 address. The host portion must be a dotted-decimal IPv4 address (DNS resolution is not supported).

**Parameters**:

- `hostPort` (`String`)
- `Address string in "host:port" format` (`e.g., "127.0.0.1:9092"`)

**Returns**: `HostAddress` — Resolved address with host, port, and packed IP fields.

**Raises**:

- `SocketError` → `Error` — If the string does not contain a colon separator, or if the host portion is not a valid IPv4 address.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

### Methods

#### `function parseHostPort( String hostPort ) -> HostAddress`

Parse a "host:port" string into its components and resolve the host to a packed IPv4 address. The host portion must be a dotted-decimal IPv4 address (DNS resolution is not supported).

**Parameters**:

- `hostPort` (`String`)
- `Address string in "host:port" format` (`e.g., "127.0.0.1:9092"`)

**Returns**: `HostAddress` — Resolved address with host, port, and packed IP fields.

**Raises**:

- `SocketError` → `Error` — If the string does not contain a colon separator, or if the host portion is not a valid IPv4 address.

**Complexity**:
- Time: `O(n) where n is string length`
- Space: `O(1)`

