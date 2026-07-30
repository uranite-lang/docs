# uranite.os.net.socket

## Table of Contents

- [Imports](#imports)
- [const `SOCKET_STREAM`](#const-socket-stream)
- [const `SOCKET_DATAGRAM`](#const-socket-datagram)
- [const `SOCKET_RAW`](#const-socket-raw)
- [const `SOCKET_UNBOUND`](#const-socket-unbound)
- [const `SOCKET_BOUND`](#const-socket-bound)
- [const `SOCKET_LISTENING`](#const-socket-listening)
- [const `SOCKET_CONNECTED`](#const-socket-connected)
- [const `SOCKET_CLOSED`](#const-socket-closed)
- [const `AF_INET`](#const-af-inet)
- [const `AF_INET6`](#const-af-inet6)
- [class `SocketTable`](#class-sockettable)
  - [`SocketTable()`](#SocketTable)
  - [`create()`](#create)
  - [`bind()`](#bind)
  - [`listen()`](#listen)
  - [`connect()`](#connect)
  - [`close()`](#close)
  - [`getState()`](#getState)
  - [`getType()`](#getType)
  - [`getLocalPort()`](#getLocalPort)
  - [`getRemoteAddr()`](#getRemoteAddr)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## const `SOCKET_STREAM`

Socket type constant for stream sockets. Stream sockets provide reliable, ordered, connection-oriented byte stream delivery and map to TCP.

## const `SOCKET_DATAGRAM`

Socket type constant for datagram sockets. Datagram sockets provide connectionless, unreliable message delivery and map to UDP.

## const `SOCKET_RAW`

Socket type constant for raw sockets. Raw sockets bypass the transport layer and provide direct access to the network layer, allowing applications to construct and receive raw IP packets.

## const `SOCKET_UNBOUND`

Socket state for an unbound socket. Initial state after socket creation, before any local address has been assigned.

## const `SOCKET_BOUND`

Socket state for a bound socket. A socket enters this state after successfully binding to a local address and port.

## const `SOCKET_LISTENING`

Socket state for a listening socket. A stream socket enters this state after calling listen, making it ready to accept incoming connections.

## const `SOCKET_CONNECTED`

Socket state for a connected socket. A socket enters this state after successfully connecting to a remote address and port.

## const `SOCKET_CLOSED`

Socket state for a closed socket. A socket enters this state after being explicitly closed and is no longer usable.

## const `AF_INET`

Address family constant for IPv4, corresponding to the AF_INET address family used in the BSD socket API.

## const `AF_INET6`

Address family constant for IPv6, corresponding to the AF_INET6 address family used in the BSD socket API.

## class `SocketTable`

Manages all open sockets in the kernel networking stack. The socket table tracks the complete lifecycle of each socket from creation through binding, listening or connecting, and finally closing. Each socket is identified by a unique monotonically increasing ID. The table uses parallel arrays for socket metadata (type, state, local and remote addresses/ports, protocol, and backing slot ID) and a spinlock for thread-safe concurrent access.

### Fields

| Name | Type | Access |
|------|------|--------|
| `types` | `Memory<I64>` | public |
| `states` | `Memory<I64>` | public |
| `localAddrs` | `Memory<I64>` | public |
| `localPorts` | `Memory<I64>` | public |
| `remoteAddrs` | `Memory<I64>` | public |
| `remotePorts` | `Memory<I64>` | public |
| `protocols` | `Memory<I64>` | public |
| `backingSlots` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `count` | `I64` | public |
| `nextId` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function SocketTable( self, I64 maxSockets ) -> Void`

Constructs a new socket table with the specified maximum number of sockets. All slots are initialized to empty, the next socket ID starts at 1, and a spinlock is created for concurrent access protection.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function create( self, I64 sockType, I64 protocol ) -> I64`

Creates a new socket of the specified type and protocol. Allocates the first available slot in the table, assigns a unique monotonically increasing socket ID, and initializes the socket in the unbound state. Returns the socket ID (1-based) on success, or -1 if the table is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findSlot( self, I64 socketId ) -> I64`

Searches the socket table for the internal slot index corresponding to the given socket ID. Returns the slot index if found, or -1 if no active socket with the given ID exists.

#### `function bind( self, I64 socketId, I64 addr, I64 port ) -> Boolean`

Binds the socket identified by socketId to the specified local address and port. The socket must be in the unbound state (state 0) for binding to succeed. Returns True if the socket was successfully bound, or False if the socket was not found or is not in the unbound state.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function listen( self, I64 socketId ) -> Boolean`

Transitions the socket identified by socketId to the listening state, making it ready to accept incoming connections. The socket must be a stream socket (type 1) in the bound state (state 1). Returns True on success, or False if the socket was not found, is not bound, or is not a stream socket.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function connect( self, I64 socketId, I64 remoteAddr, I64 remotePort ) -> Boolean`

Connects the socket identified by socketId to the specified remote address and port. The socket must be in either the unbound or bound state (state 0 or 1). Returns True if the connection was established, or False if the socket was not found or is already listening or connected.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function close( self, I64 socketId ) -> Boolean`

Closes the socket identified by socketId, releasing its slot in the socket table. All associated state (type, addresses, ports, protocol) is cleared. Returns True if the socket was found and closed, or False if no socket with the given ID exists.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getState( self, I64 socketId ) -> I64`

Returns the current state of the socket identified by socketId. Returns -1 if no socket with the given ID exists.

#### `function getType( self, I64 socketId ) -> I64`

Returns the type of the socket identified by socketId (stream, datagram, or raw). Returns 0 if no socket with the given ID exists.

#### `function getLocalPort( self, I64 socketId ) -> I64`

Returns the local port number bound to the socket identified by socketId. Returns 0 if no socket with the given ID exists.

#### `function getRemoteAddr( self, I64 socketId ) -> I64`

Returns the remote address of the socket identified by socketId. Returns 0 if no socket with the given ID exists.

#### `function getCount( self ) -> I64`

Returns the number of currently open sockets in the socket table. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the socket table, including the types, states, address, port, protocol, and backing slot arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

