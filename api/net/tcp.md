# uranite.net.tcp

## Table of Contents

- [Imports](#imports)
- [class `TcpSocket`](#class-tcpsocket)
  - [`TcpSocket()`](#TcpSocket)
  - [`TcpSocket()`](#TcpSocket)
  - [`connect()`](#connect)
  - [`connect()`](#connect)
  - [`send()`](#send)
  - [`receive()`](#receive)
  - [`setNoDelay()`](#setNoDelay)
  - [`setAddressReuse()`](#setAddressReuse)
  - [`close()`](#close)
  - [`destroy()`](#destroy)
- [class `TcpListener`](#class-tcplistener)
  - [`TcpListener()`](#TcpListener)
  - [`accept()`](#accept)
  - [`close()`](#close)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.syscall`
  - `memoryToPtr`
  - `writeI32At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.net.address`
  - `HostAddress`
  - `parseHostPort`
  - `parseIpv4`
- `uranite.net.errors`
  - `SocketError`
- `uranite.net.socket`
  - `buildSockaddrIn`
  - `createTcpSocket`
  - `htons`
  - `ipToInt`
  - `recvString`
  - `sendString`
  - `setReuseAddr`
  - `setReusePort`
  - `setTcpNoDelay`
  - `socketAccept`
  - `socketClose`
  - `socketShutdown`
  - `tcpConnect`
  - `tcpListen`

## class `TcpSocket`

Connected TCP socket for bidirectional stream communication.

Wraps a Linux TCP file descriptor with typed send/receive operations and socket option methods. Tracks connection state to prevent operations on closed sockets.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fileDescriptor` | `I64` | public |
| `connected` | `Boolean` | public |

### Methods

#### `function TcpSocket( self ) -> Void`

Create an unconnected TCP socket.

Allocates a new AF_INET SOCK_STREAM socket via the socket syscall. The socket must be connected via connect() before send/receive.

**Raises**:

- `SocketError` → `Error` — If the socket cannot be created.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function TcpSocket( self, I64 existingFd ) -> Void`

Wrap an existing connected file descriptor as a TcpSocket.

Used internally by TcpListener.accept() to wrap accepted connections.

**Parameters**:

- `existingFd` (`I64`)
- `An already-connected socket file descriptor.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function connect( self, String address ) -> Void`

Connect this socket to a remote endpoint specified as a "host:port" string. The host portion is parsed as a dotted-decimal IPv4 address and resolved internally.

**Parameters**:

- `address` (`String`)
- `Endpoint in "host:port" format` (`e.g., "127.0.0.1:8080"`)

**Raises**:

- `SocketError` → `Error` — If the socket is already connected, the address format is invalid, or the connection cannot be established.

**Complexity**:
- Time: `O(1) (network latency notwithstanding)`
- Space: `O(1)`

#### `function connect( self, String host, I64 port ) -> Void`

Connect this socket to a remote host at the given address and port. The host string is parsed as a dotted-decimal IPv4 address and resolved internally.

**Parameters**:

- `host` (`String`)
- `IPv4 address as dotted-decimal string` (`e.g., "127.0.0.1"`)
- `port` (`I64`)
- `The TCP port number to connect to.`

**Raises**:

- `SocketError` → `Error` — If the socket is already connected, the host address is invalid, or the connection cannot be established.

**Complexity**:
- Time: `O(1) (network latency notwithstanding)`
- Space: `O(1)`

#### `function connectRaw( self, I64 ipAddress, I64 port ) -> Void`

Connect using a pre-resolved packed IPv4 address integer. Internal use only — prefer connect(String, I64) for public API.

**Parameters**:

- `ipAddress` (`I64`)
- `The IPv4 address as a packed 32-bit integer.`
- `port` (`I64`)
- `The TCP port number to connect to.`

**Raises**:

- `SocketError` → `Error` — If the socket is already connected or connection fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function send( self, String data ) -> I64`

Send a string over the connected socket.

**Parameters**:

- `data` (`String`)
- `The data to send.`

**Returns**: `I64` — The number of bytes actually sent.

**Raises**:

- `SocketError` → `Error` — If the socket is not connected or the send fails.

**Complexity**:
- Time: `O(n) where n is the data length`

#### `function receive( self, I64 maxLength ) -> String`

Receive up to maxLength bytes from the connected socket.

Returns an empty string on EOF (remote side closed).

**Parameters**:

- `maxLength` (`I64`)
- `Maximum number of bytes to receive.`

**Returns**: `String` — The received data, or empty string on connection close.

**Raises**:

- `SocketError` → `Error` — If the socket is not connected or the receive fails.

**Complexity**:
- Time: `O(n) where n is the received data length`

#### `function setNoDelay( self ) -> Void`

Enable TCP_NODELAY to disable Nagle's algorithm.

This reduces latency for small messages at the cost of potentially increased network overhead.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setAddressReuse( self ) -> Void`

Enable SO_REUSEADDR on this socket.

Allows rebinding to a recently-used address without waiting for the TIME_WAIT period to expire.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function close( self ) -> Void`

Close the socket and release the file descriptor.

After closing, no further send or receive operations are possible.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Destructor that ensures the socket is closed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `TcpListener`

TCP server socket that binds to a port and accepts incoming connections.

Wraps the bind-listen-accept sequence into a simple API. Each accepted connection is returned as a connected TcpSocket instance.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fileDescriptor` | `I64` | public |
| `listening` | `Boolean` | public |

### Methods

#### `function TcpListener( self, I64 port, I64 backlog ) -> Void`

Create a TCP listener bound to the given port.

Binds to all interfaces (0.0.0.0) and begins listening with the specified connection backlog size. Enables SO_REUSEADDR automatically.

**Parameters**:

- `port` (`I64`)
- `The TCP port number to listen on.`
- `backlog` (`I64`)
- `Maximum number of pending connections in the queue.`

**Raises**:

- `SocketError` → `Error` — If binding or listening fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function accept( self ) -> TcpSocket`

Block until an incoming connection arrives, then return a connected socket.

**Returns**: `TcpSocket` — A new TcpSocket wrapping the accepted connection.

**Raises**:

- `SocketError` → `Error` — If the listener is not active or the accept fails.

**Complexity**:
- Time: `O(1) (blocks until connection arrives)`
- Space: `O(1)`

#### `function close( self ) -> Void`

Stop listening and close the server socket.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Destructor that ensures the listener socket is closed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

