# uranite.os.net.udp

## Table of Contents

- [Imports](#imports)
- [const `UDP_HEADER_SIZE`](#const-udp-header-size)
- [class `UdpHeader`](#class-udpheader)
  - [`UdpHeader()`](#UdpHeader)
  - [`configure()`](#configure)
  - [`getPayloadLength()`](#getPayloadLength)
- [class `UdpBindTable`](#class-udpbindtable)
  - [`UdpBindTable()`](#UdpBindTable)
  - [`bind()`](#bind)
  - [`unbind()`](#unbind)
  - [`findListener()`](#findListener)
  - [`isBound()`](#isBound)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## const `UDP_HEADER_SIZE`

UDP datagram header size in bytes. Four 16-bit fields: source port, dest port, length, checksum. 

## class `UdpHeader`

Represents a UDP datagram header as defined in RFC 768. The header is exactly 8 bytes and contains the source port, destination port, total datagram length (header plus payload), and a checksum for integrity verification. UDP provides connectionless, unreliable datagram delivery with minimal protocol overhead.

### Fields

| Name | Type | Access |
|------|------|--------|
| `srcPort` | `I64` | public |
| `dstPort` | `I64` | public |
| `length` | `I64` | public |
| `checksum` | `I64` | public |

### Methods

#### `function UdpHeader( self ) -> Void`

Constructs a new UDP header with ports set to zero, length set to 8 (header only, no payload), and checksum set to zero.

#### `function configure( self, I64 src, I64 dst, I64 payloadLen ) -> Void`

Configures this UDP header with the specified source port, destination port, and payload length. The total datagram length is automatically calculated as 8 (header size) plus the payload length.

#### `function getPayloadLength( self ) -> I64`

Returns the payload length in bytes by subtracting the 8-byte header size from the total datagram length.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `UdpBindTable`

Maps UDP port numbers to listener identifiers for incoming datagram delivery. When a UDP datagram arrives, the kernel looks up the destination port in this table to determine which listener should receive the data. Each binding associates a port number with a listener ID and an optional local address filter, where an address of 0 matches any local address (wildcard binding).

### Fields

| Name | Type | Access |
|------|------|--------|
| `ports` | `Memory<I64>` | public |
| `listenerIds` | `Memory<I64>` | public |
| `addresses` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `count` | `I64` | public |

### Methods

#### `function UdpBindTable( self, I64 maxBindings ) -> Void`

Constructs a new UDP binding table with the specified maximum number of port bindings. All slots are initialized to empty.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function bind( self, I64 port, I64 listenerId, I64 addr ) -> Boolean`

Binds a port to the specified listener ID with an optional local address filter. An address of 0 creates a wildcard binding that accepts datagrams on any local address. Returns False if the port is already bound to another listener or if the table is full. Returns True on successful binding.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unbind( self, I64 port ) -> Boolean`

Removes the binding for the specified port, freeing the slot for reuse. Returns True if the port was bound and has been successfully unbound, or False if no binding exists for the given port.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findListener( self, I64 port, I64 addr ) -> I64`

Finds the listener ID for an incoming datagram addressed to the specified port and local address. A binding with address 0 (wildcard) matches any destination address. Returns the listener ID if a matching binding is found, or 0 if no listener is bound to the port.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function isBound( self, I64 port ) -> Boolean`

Checks whether the specified port has an active binding in the table. Returns True if a listener is bound to the port, or False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Returns the number of active port bindings in the UDP binding table. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the UDP binding table, including the ports, listener IDs, and addresses arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

