# uranite.os.net.tcp

## Table of Contents

- [Imports](#imports)
- [const `TCP_FIN`](#const-tcp-fin)
- [const `TCP_SYN`](#const-tcp-syn)
- [const `TCP_RST`](#const-tcp-rst)
- [const `TCP_PSH`](#const-tcp-psh)
- [const `TCP_ACK`](#const-tcp-ack)
- [const `TCP_URG`](#const-tcp-urg)
- [enum `TcpState`](#enum-tcpstate)
- [const `TCP_HEADER_SIZE`](#const-tcp-header-size)
- [class `TcpHeader`](#class-tcpheader)
  - [`TcpHeader()`](#TcpHeader)
  - [`isSyn()`](#isSyn)
  - [`isAck()`](#isAck)
  - [`isFin()`](#isFin)
  - [`isRst()`](#isRst)
  - [`isPsh()`](#isPsh)
  - [`isSynAck()`](#isSynAck)
  - [`setFlag()`](#setFlag)
  - [`clearFlags()`](#clearFlags)
- [class `TcpConnectionTable`](#class-tcpconnectiontable)
  - [`TcpConnectionTable()`](#TcpConnectionTable)
  - [`create()`](#create)
  - [`find()`](#find)
  - [`findListener()`](#findListener)
  - [`getState()`](#getState)
  - [`setState()`](#setState)
  - [`updateSeq()`](#updateSeq)
  - [`close()`](#close)
  - [`getCount()`](#getCount)
  - [`getSeqNumber()`](#getSeqNumber)
  - [`getAckNumber()`](#getAckNumber)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## const `TCP_FIN`

TCP FIN flag bitmask. Sender finished sending data, wishes to close connection. 

## const `TCP_SYN`

TCP SYN flag bitmask. Used during three-way handshake to synchronize sequence numbers. 

## const `TCP_RST`

TCP RST flag bitmask. Forcibly resets a connection or aborts an existing one. 

## const `TCP_PSH`

TCP PSH flag bitmask. Requests immediate delivery of buffered data to application. 

## const `TCP_ACK`

TCP ACK flag bitmask. Acknowledgment number field is valid. Set on all post-SYN segments. 

## const `TCP_URG`

TCP URG flag bitmask. Urgent pointer field is significant, out-of-band data present. 

## enum `TcpState`

Enumerates the TCP connection states as defined in RFC 793. These states model the full TCP connection lifecycle from the initial closed state through the three-way handshake (SYN, SYN-ACK, ACK), established data transfer, and the four-way teardown (FIN, ACK, FIN, ACK) including the TIME_WAIT state.

## const `TCP_HEADER_SIZE`

Minimum TCP segment header size in bytes. Data offset 5 (5 x 4 = 20) with no options. 

## class `TcpHeader`

Represents a TCP segment header as defined in RFC 793. The minimum header size is 20 bytes and includes source port, destination port, 32-bit sequence number, 32-bit acknowledgment number, data offset (header length in 32-bit words), control flags (URG, ACK, PSH, RST, SYN, FIN), window size for flow control, checksum, and urgent pointer.

### Fields

| Name | Type | Access |
|------|------|--------|
| `srcPort` | `I64` | public |
| `dstPort` | `I64` | public |
| `sequenceNumber` | `I64` | public |
| `ackNumber` | `I64` | public |
| `dataOffset` | `I64` | public |
| `flags` | `I64` | public |
| `windowSize` | `I64` | public |
| `checksum` | `I64` | public |
| `urgentPointer` | `I64` | public |

### Methods

#### `function TcpHeader( self ) -> Void`

Constructs a new TCP header with default values: data offset of 5 (20 bytes, no options), maximum window size of 65535 bytes, and all other fields set to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isSyn( self ) -> Boolean`

Returns True if the SYN flag (bit 1) is set in this segment's flags. 

#### `function isAck( self ) -> Boolean`

Returns True if the ACK flag (bit 4) is set in this segment's flags. 

#### `function isFin( self ) -> Boolean`

Returns True if the FIN flag (bit 0) is set in this segment's flags. 

#### `function isRst( self ) -> Boolean`

Returns True if the RST flag (bit 2) is set in this segment's flags. 

#### `function isPsh( self ) -> Boolean`

Returns True if the PSH flag (bit 3) is set in this segment's flags. 

#### `function isSynAck( self ) -> Boolean`

Returns True if both the SYN and ACK flags are set in this segment's flags. A SYN-ACK segment is the second step of the TCP three-way handshake, sent by the server in response to the client's initial SYN.

#### `function setFlag( self, I64 flag ) -> Void`

Sets the specified flag bit in the segment's flags field using a bitwise OR operation. Multiple flags can be set by calling this method repeatedly.

#### `function clearFlags( self ) -> Void`

Clears all control flags in this TCP header by resetting the flags field to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `TcpConnectionTable`

Tracks active TCP connections in the kernel networking stack. Each connection is identified by its 4-tuple of local address, local port, remote address, and remote port. The table stores per-connection state including the TCP state machine state, sequence and acknowledgment numbers, and receive window size. A spinlock protects concurrent access from interrupt handlers and kernel threads.

### Fields

| Name | Type | Access |
|------|------|--------|
| `localAddrs` | `Memory<I64>` | public |
| `localPorts` | `Memory<I64>` | public |
| `remoteAddrs` | `Memory<I64>` | public |
| `remotePorts` | `Memory<I64>` | public |
| `states` | `Memory<I64>` | public |
| `seqNumbers` | `Memory<I64>` | public |
| `ackNumbers` | `Memory<I64>` | public |
| `windows` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `count` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function TcpConnectionTable( self, I64 maxConnections ) -> Void`

Constructs a new TCP connection table with the specified maximum number of simultaneous connections. All slots are initialized to the closed state (0) with zeroed address and port fields.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function create( self, I64 localAddr, I64 localPort, I64 remoteAddr, I64 remotePort, I64 state ) -> I64`

Creates a new connection entry in the table with the specified 4-tuple (local address, local port, remote address, remote port) and initial TCP state. The window size is initialized to 65535 bytes. Returns the slot index on success, or -1 if the table is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function find( self, I64 localAddr, I64 localPort, I64 remoteAddr, I64 remotePort ) -> I64`

Searches for a connection matching the given 4-tuple of local address, local port, remote address, and remote port. Only active connections (state not equal to 0) are considered. Returns the slot index if found, or -1 if no matching connection exists.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findListener( self, I64 localPort ) -> I64`

Searches for a listening socket (state 1 = Listen) bound to the specified local port. This is used during incoming connection handling to find the server socket that should accept the connection. Returns the slot index if found, or -1 otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getState( self, I64 slot ) -> I64`

Returns the TCP state of the connection at the specified slot index. Returns 0 (Closed) if the slot index is out of bounds.

#### `function setState( self, I64 slot, I64 state ) -> Void`

Sets the TCP state of the connection at the specified slot index. The state value corresponds to TcpState enum values. No action is taken if the slot is out of bounds.

#### `function updateSeq( self, I64 slot, I64 seq, I64 ack ) -> Void`

Updates the sequence number and acknowledgment number for the connection at the specified slot index. These values track the TCP byte stream positions for both directions of the connection. No action is taken if the slot is out of bounds.

#### `function close( self, I64 slot ) -> Boolean`

Closes the connection at the specified slot index by resetting its state to Closed and clearing all associated address, port, and sequence number fields. The slot is freed for reuse. Returns True if the connection was successfully closed, or False if the slot is out of bounds or already closed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Returns the number of active connections currently tracked in the table. 

#### `function getSeqNumber( self, I64 slot ) -> I64`

Returns the current sequence number for the connection at the specified slot index. Returns 0 if the slot is out of bounds.

#### `function getAckNumber( self, I64 slot ) -> I64`

Returns the current acknowledgment number for the connection at the specified slot index. Returns 0 if the slot is out of bounds.

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the TCP connection table, including address, port, state, sequence number, acknowledgment number, and window size arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

