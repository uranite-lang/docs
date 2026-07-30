# uranite.net.socket

## Table of Contents

- [Imports](#imports)
- [const `SYS_SOCKET`](#const-sys-socket)
- [const `SYS_CONNECT`](#const-sys-connect)
- [const `SYS_ACCEPT`](#const-sys-accept)
- [const `SYS_SENDTO`](#const-sys-sendto)
- [const `SYS_RECVFROM`](#const-sys-recvfrom)
- [const `SYS_BIND`](#const-sys-bind)
- [const `SYS_LISTEN`](#const-sys-listen)
- [const `SYS_SETSOCKOPT`](#const-sys-setsockopt)
- [const `SYS_GETSOCKNAME`](#const-sys-getsockname)
- [const `SYS_GETPEERNAME`](#const-sys-getpeername)
- [const `SYS_SHUTDOWN`](#const-sys-shutdown)
- [const `AF_INET`](#const-af-inet)
- [const `AF_INET6`](#const-af-inet6)
- [const `SOCK_STREAM`](#const-sock-stream)
- [const `SOCK_DGRAM`](#const-sock-dgram)
- [const `SOCK_NONBLOCK`](#const-sock-nonblock)
- [const `IPPROTO_TCP`](#const-ipproto-tcp)
- [const `IPPROTO_UDP`](#const-ipproto-udp)
- [const `SOL_SOCKET`](#const-sol-socket)
- [const `SO_REUSEADDR`](#const-so-reuseaddr)
- [const `SO_REUSEPORT`](#const-so-reuseport)
- [const `SO_KEEPALIVE`](#const-so-keepalive)
- [const `SO_RCVTIMEO`](#const-so-rcvtimeo)
- [const `SO_SNDTIMEO`](#const-so-sndtimeo)
- [const `TCP_NODELAY`](#const-tcp-nodelay)
- [const `SOL_TCP`](#const-sol-tcp)
- [const `SHUT_RD`](#const-shut-rd)
- [const `SHUT_WR`](#const-shut-wr)
- [const `SHUT_RDWR`](#const-shut-rdwr)
- [const `SOCKADDR_IN_SIZE`](#const-sockaddr-in-size)
- [const `INADDR_ANY`](#const-inaddr-any)
- [const `INADDR_LOOPBACK`](#const-inaddr-loopback)
- [const `ECONNREFUSED`](#const-econnrefused)
- [const `EADDRINUSE`](#const-eaddrinuse)
- [const `ECONNRESET`](#const-econnreset)
- [function `htons`](#function-htons)
  - [`htons()`](#htons)
- [function `ntohs`](#function-ntohs)
  - [`ntohs()`](#ntohs)
- [function `ipToInt`](#function-iptoint)
  - [`ipToInt()`](#ipToInt)
- [function `buildSockaddrIn`](#function-buildsockaddrin)
  - [`buildSockaddrIn()`](#buildSockaddrIn)
- [function `socketCreate`](#function-socketcreate)
  - [`socketCreate()`](#socketCreate)
- [function `socketBind`](#function-socketbind)
  - [`socketBind()`](#socketBind)
- [function `socketListen`](#function-socketlisten)
  - [`socketListen()`](#socketListen)
- [function `socketAccept`](#function-socketaccept)
  - [`socketAccept()`](#socketAccept)
- [function `socketConnect`](#function-socketconnect)
  - [`socketConnect()`](#socketConnect)
- [function `socketSend`](#function-socketsend)
  - [`socketSend()`](#socketSend)
- [function `socketRecv`](#function-socketrecv)
  - [`socketRecv()`](#socketRecv)
- [function `socketClose`](#function-socketclose)
  - [`socketClose()`](#socketClose)
- [function `socketSetOpt`](#function-socketsetopt)
  - [`socketSetOpt()`](#socketSetOpt)
- [function `socketShutdown`](#function-socketshutdown)
  - [`socketShutdown()`](#socketShutdown)
- [function `setReuseAddr`](#function-setreuseaddr)
  - [`setReuseAddr()`](#setReuseAddr)
- [function `setReusePort`](#function-setreuseport)
  - [`setReusePort()`](#setReusePort)
- [function `setTcpNoDelay`](#function-settcpnodelay)
  - [`setTcpNoDelay()`](#setTcpNoDelay)
- [function `sendString`](#function-sendstring)
  - [`sendString()`](#sendString)
- [function `recvString`](#function-recvstring)
  - [`recvString()`](#recvString)
- [function `createTcpSocket`](#function-createtcpsocket)
  - [`createTcpSocket()`](#createTcpSocket)
- [function `createUdpSocket`](#function-createudpsocket)
  - [`createUdpSocket()`](#createUdpSocket)
- [function `tcpListen`](#function-tcplisten)
  - [`tcpListen()`](#tcpListen)
- [function `tcpConnect`](#function-tcpconnect)
  - [`tcpConnect()`](#tcpConnect)

## Imports

- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `SYS_CLOSE`
  - `memoryToPtr`
  - `readByteAt`
  - `readI16At`
  - `readI32At`
  - `readI64At`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
  - `writeI16At`
  - `writeI32At`
  - `writeI64At`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`
- `uranite.memory.memory`
  - `Memory`
- `uranite.net.errors`
  - `AddressInUseError`
  - `ConnectionError`
  - `ConnectionRefusedError`
  - `SocketError`
- `uranite.os.syscall.invoke`
  - `syscall1`
  - `syscall2`
  - `syscall3`
  - `syscall4`
  - `syscall5`
  - `syscall6`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `SYS_SOCKET`

Linux x86_64 syscall number for socket(2).

## const `SYS_CONNECT`

Linux x86_64 syscall number for connect(2).

## const `SYS_ACCEPT`

Linux x86_64 syscall number for accept(2).

## const `SYS_SENDTO`

Linux x86_64 syscall number for sendto(2).

## const `SYS_RECVFROM`

Linux x86_64 syscall number for recvfrom(2).

## const `SYS_BIND`

Linux x86_64 syscall number for bind(2).

## const `SYS_LISTEN`

Linux x86_64 syscall number for listen(2).

## const `SYS_SETSOCKOPT`

Linux x86_64 syscall number for setsockopt(2).

## const `SYS_GETSOCKNAME`

Linux x86_64 syscall number for getsockname(2).

## const `SYS_GETPEERNAME`

Linux x86_64 syscall number for getpeername(2).

## const `SYS_SHUTDOWN`

Linux x86_64 syscall number for shutdown(2).

## const `AF_INET`

IPv4 address family constant.

## const `AF_INET6`

IPv6 address family constant.

## const `SOCK_STREAM`

Stream socket type for TCP connections.

## const `SOCK_DGRAM`

Datagram socket type for UDP communication.

## const `SOCK_NONBLOCK`

Flag to create a non-blocking socket.

## const `IPPROTO_TCP`

Protocol number for TCP.

## const `IPPROTO_UDP`

Protocol number for UDP.

## const `SOL_SOCKET`

Socket-level option for setsockopt.

## const `SO_REUSEADDR`

Socket option to allow address reuse.

## const `SO_REUSEPORT`

Socket option to allow port reuse across multiple sockets.

## const `SO_KEEPALIVE`

Socket option to enable TCP keepalive probes.

## const `SO_RCVTIMEO`

Socket option to set the receive timeout.

## const `SO_SNDTIMEO`

Socket option to set the send timeout.

## const `TCP_NODELAY`

TCP option to disable Nagle's algorithm for low-latency sends.

## const `SOL_TCP`

TCP-level option for setsockopt.

## const `SHUT_RD`

Shutdown flag to disable further reads on the socket.

## const `SHUT_WR`

Shutdown flag to disable further writes on the socket.

## const `SHUT_RDWR`

Shutdown flag to disable both reads and writes on the socket.

## const `SOCKADDR_IN_SIZE`

Size in bytes of a sockaddr_in structure for IPv4.

## const `INADDR_ANY`

IPv4 address constant representing all local interfaces (0.0.0.0).

## const `INADDR_LOOPBACK`

IPv4 address constant representing the loopback interface (127.0.0.1).

## const `ECONNREFUSED`

Linux errno for connection refused by remote host.

## const `EADDRINUSE`

Linux errno for address already in use.

## const `ECONNRESET`

Linux errno for connection reset by peer.

## function `htons`

Convert a 16-bit port number from host byte order to network byte order (big-endian). Swaps the high and low bytes of the port value.

**Parameters**:

- `port` (`I64`)
- `The port number in host byte order.`

**Returns**: — The port number in network byte order.

### Methods

#### `function htons( I64 port ) -> I64`

Convert a 16-bit port number from host byte order to network byte order (big-endian). Swaps the high and low bytes of the port value.

**Parameters**:

- `port` (`I64`)
- `The port number in host byte order.`

**Returns**: — The port number in network byte order.

## function `ntohs`

Convert a 16-bit port number from network byte order (big-endian) to host byte order. This is the inverse of htons and uses the same byte swap operation since the transformation is symmetric.

**Parameters**:

- `netPort` (`I64`)
- `The port number in network byte order.`

**Returns**: — The port number in host byte order.

### Methods

#### `function ntohs( I64 netPort ) -> I64`

Convert a 16-bit port number from network byte order (big-endian) to host byte order. This is the inverse of htons and uses the same byte swap operation since the transformation is symmetric.

**Parameters**:

- `netPort` (`I64`)
- `The port number in network byte order.`

**Returns**: — The port number in host byte order.

## function `ipToInt`

Convert an IPv4 address from four individual octets into a single 32-bit integer in network byte order suitable for use in sockaddr_in structures.

**Parameters**:

- `octet1` (`I64`)
- `The first octet of the IPv4 address.`
- `octet2` (`I64`)
- `The second octet of the IPv4 address.`
- `octet3` (`I64`)
- `The third octet of the IPv4 address.`
- `octet4` (`I64`)
- `The fourth octet of the IPv4 address.`

**Returns**: — The IPv4 address as a 32-bit integer in network byte order.

### Methods

#### `function ipToInt( I64 octet1, I64 octet2, I64 octet3, I64 octet4 ) -> I64`

Convert an IPv4 address from four individual octets into a single 32-bit integer in network byte order suitable for use in sockaddr_in structures.

**Parameters**:

- `octet1` (`I64`)
- `The first octet of the IPv4 address.`
- `octet2` (`I64`)
- `The second octet of the IPv4 address.`
- `octet3` (`I64`)
- `The third octet of the IPv4 address.`
- `octet4` (`I64`)
- `The fourth octet of the IPv4 address.`

**Returns**: — The IPv4 address as a 32-bit integer in network byte order.

## function `buildSockaddrIn`

Allocate and populate a sockaddr_in structure for IPv4 with the given port and address. The caller is responsible for deallocating the returned buffer with dealloc when it is no longer needed.

**Parameters**:

- `port` (`I64`)
- `The port number in host byte order` (`will be converted to network byte order`)
- `addr` (`I64`)
- `The IPv4 address as a 32-bit integer in network byte order.`

**Returns**: — A pointer to the allocated sockaddr_in structure.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function buildSockaddrIn( I64 port, I64 addr ) -> I64`

Allocate and populate a sockaddr_in structure for IPv4 with the given port and address. The caller is responsible for deallocating the returned buffer with dealloc when it is no longer needed.

**Parameters**:

- `port` (`I64`)
- `The port number in host byte order` (`will be converted to network byte order`)
- `addr` (`I64`)
- `The IPv4 address as a 32-bit integer in network byte order.`

**Returns**: — A pointer to the allocated sockaddr_in structure.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketCreate`

Create a new socket file descriptor via the socket(2) syscall.

**Parameters**:

- `domain` (`I64`)
- `The address family` (`e.g., AF_INET for IPv4`)
- `sockType` (`I64`)
- `The socket type` (`e.g., SOCK_STREAM for TCP, SOCK_DGRAM for UDP`)
- `protocol` (`I64`)
- `The protocol number` (`typically 0 for default protocol selection`)

**Returns**: — The file descriptor of the newly created socket.

**Raises**:

- `SocketError` → `Error` — If the socket syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketCreate( I64 domain, I64 sockType, I64 protocol ) -> I64`

Create a new socket file descriptor via the socket(2) syscall.

**Parameters**:

- `domain` (`I64`)
- `The address family` (`e.g., AF_INET for IPv4`)
- `sockType` (`I64`)
- `The socket type` (`e.g., SOCK_STREAM for TCP, SOCK_DGRAM for UDP`)
- `protocol` (`I64`)
- `The protocol number` (`typically 0 for default protocol selection`)

**Returns**: — The file descriptor of the newly created socket.

**Raises**:

- `SocketError` → `Error` — If the socket syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketBind`

Bind a socket to a local address via the bind(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to bind.`
- `addrPtr` (`I64`)
- `A pointer to the sockaddr structure containing the address to bind to.`
- `addrLen` (`I64`)
- `The size of the sockaddr structure in bytes.`

**Raises**:

- `AddressInUseError` → `SocketError` → `Error` — If the address is already in use (EADDRINUSE).
- `SocketError` → `Error` — If the bind syscall fails for any other reason.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketBind( I64 fd, I64 addrPtr, I64 addrLen ) -> Void`

Bind a socket to a local address via the bind(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to bind.`
- `addrPtr` (`I64`)
- `A pointer to the sockaddr structure containing the address to bind to.`
- `addrLen` (`I64`)
- `The size of the sockaddr structure in bytes.`

**Raises**:

- `AddressInUseError` → `SocketError` → `Error` — If the address is already in use (EADDRINUSE).
- `SocketError` → `Error` — If the bind syscall fails for any other reason.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketListen`

Mark a socket as passive and ready to accept incoming connections via the listen(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to listen on.`
- `backlog` (`I64`)
- `The maximum number of pending connections in the listen queue.`

**Raises**:

- `SocketError` → `Error` — If the listen syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketListen( I64 fd, I64 backlog ) -> Void`

Mark a socket as passive and ready to accept incoming connections via the listen(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to listen on.`
- `backlog` (`I64`)
- `The maximum number of pending connections in the listen queue.`

**Raises**:

- `SocketError` → `Error` — If the listen syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketAccept`

Accept an incoming connection on a listening socket via the accept(2) syscall. Returns a new file descriptor for the accepted connection.

**Parameters**:

- `fd` (`I64`)
- `The listening socket file descriptor.`
- `addrPtr` (`I64`)
- `A pointer to a sockaddr structure to receive the client address, or 0 to ignore.`
- `addrLenPtr` (`I64`)
- `A pointer to an integer containing the size of the address buffer, or 0 to ignore.`

**Returns**: — The file descriptor of the newly accepted connection.

**Raises**:

- `SocketError` → `Error` — If the accept syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketAccept( I64 fd, I64 addrPtr, I64 addrLenPtr ) -> I64`

Accept an incoming connection on a listening socket via the accept(2) syscall. Returns a new file descriptor for the accepted connection.

**Parameters**:

- `fd` (`I64`)
- `The listening socket file descriptor.`
- `addrPtr` (`I64`)
- `A pointer to a sockaddr structure to receive the client address, or 0 to ignore.`
- `addrLenPtr` (`I64`)
- `A pointer to an integer containing the size of the address buffer, or 0 to ignore.`

**Returns**: — The file descriptor of the newly accepted connection.

**Raises**:

- `SocketError` → `Error` — If the accept syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketConnect`

Initiate a connection on a socket to a remote address via the connect(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to connect.`
- `addrPtr` (`I64`)
- `A pointer to the sockaddr structure containing the target address.`
- `addrLen` (`I64`)
- `The size of the sockaddr structure in bytes.`

**Raises**:

- `ConnectionRefusedError` → `ConnectionError` → `SocketError` → `Error` — If the remote host refuses the connection (ECONNREFUSED).
- `ConnectionError` → `SocketError` → `Error` — If the connect syscall fails for any other reason.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketConnect( I64 fd, I64 addrPtr, I64 addrLen ) -> Void`

Initiate a connection on a socket to a remote address via the connect(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to connect.`
- `addrPtr` (`I64`)
- `A pointer to the sockaddr structure containing the target address.`
- `addrLen` (`I64`)
- `The size of the sockaddr structure in bytes.`

**Raises**:

- `ConnectionRefusedError` → `ConnectionError` → `SocketError` → `Error` — If the remote host refuses the connection (ECONNREFUSED).
- `ConnectionError` → `SocketError` → `Error` — If the connect syscall fails for any other reason.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketSend`

Send data through a connected socket via the sendto(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to send data on.`
- `bufPtr` (`I64`)
- `A pointer to the buffer containing the data to send.`
- `len` (`I64`)
- `The number of bytes to send from the buffer.`
- `flags` (`I64`)
- `Flags controlling send behavior` (`typically 0 for default`)

**Returns**: — The number of bytes successfully sent.

**Raises**:

- `SocketError` → `Error` — If the send syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketSend( I64 fd, I64 bufPtr, I64 len, I64 flags ) -> I64`

Send data through a connected socket via the sendto(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to send data on.`
- `bufPtr` (`I64`)
- `A pointer to the buffer containing the data to send.`
- `len` (`I64`)
- `The number of bytes to send from the buffer.`
- `flags` (`I64`)
- `Flags controlling send behavior` (`typically 0 for default`)

**Returns**: — The number of bytes successfully sent.

**Raises**:

- `SocketError` → `Error` — If the send syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketRecv`

Receive data from a connected socket via the recvfrom(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to receive data from.`
- `bufPtr` (`I64`)
- `A pointer to the buffer where received data will be stored.`
- `len` (`I64`)
- `The maximum number of bytes to read into the buffer.`
- `flags` (`I64`)
- `Flags controlling receive behavior` (`typically 0 for default`)

**Returns**: — The number of bytes received, or 0 if the connection has been closed.

**Raises**:

- `SocketError` → `Error` — If the recv syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketRecv( I64 fd, I64 bufPtr, I64 len, I64 flags ) -> I64`

Receive data from a connected socket via the recvfrom(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to receive data from.`
- `bufPtr` (`I64`)
- `A pointer to the buffer where received data will be stored.`
- `len` (`I64`)
- `The maximum number of bytes to read into the buffer.`
- `flags` (`I64`)
- `Flags controlling receive behavior` (`typically 0 for default`)

**Returns**: — The number of bytes received, or 0 if the connection has been closed.

**Raises**:

- `SocketError` → `Error` — If the recv syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketClose`

Close a socket file descriptor via the close(2) syscall, releasing all associated kernel resources.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to close.`

**Raises**:

- `SocketError` → `Error` — If the close syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketClose( I64 fd ) -> Void`

Close a socket file descriptor via the close(2) syscall, releasing all associated kernel resources.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to close.`

**Raises**:

- `SocketError` → `Error` — If the close syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketSetOpt`

Set a socket option via the setsockopt(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to configure.`
- `level` (`I64`)
- `The protocol level at which the option resides` (`e.g., SOL_SOCKET, SOL_TCP`)
- `optName` (`I64`)
- `The option name to set` (`e.g., SO_REUSEADDR, TCP_NODELAY`)
- `valPtr` (`I64`)
- `A pointer to the option value buffer.`
- `valLen` (`I64`)
- `The size of the option value buffer in bytes.`

**Raises**:

- `SocketError` → `Error` — If the setsockopt syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketSetOpt( I64 fd, I64 level, I64 optName, I64 valPtr, I64 valLen ) -> Void`

Set a socket option via the setsockopt(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to configure.`
- `level` (`I64`)
- `The protocol level at which the option resides` (`e.g., SOL_SOCKET, SOL_TCP`)
- `optName` (`I64`)
- `The option name to set` (`e.g., SO_REUSEADDR, TCP_NODELAY`)
- `valPtr` (`I64`)
- `A pointer to the option value buffer.`
- `valLen` (`I64`)
- `The size of the option value buffer in bytes.`

**Raises**:

- `SocketError` → `Error` — If the setsockopt syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `socketShutdown`

Shut down part or all of a full-duplex connection on a socket via the shutdown(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to shut down.`
- `how` (`I64`)
- `The shutdown mode: SHUT_RD` (`0`)
- `disable writes, or SHUT_RDWR` (`2`)

**Raises**:

- `SocketError` → `Error` — If the shutdown syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function socketShutdown( I64 fd, I64 how ) -> Void`

Shut down part or all of a full-duplex connection on a socket via the shutdown(2) syscall.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to shut down.`
- `how` (`I64`)
- `The shutdown mode: SHUT_RD` (`0`)
- `disable writes, or SHUT_RDWR` (`2`)

**Raises**:

- `SocketError` → `Error` — If the shutdown syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `setReuseAddr`

Enable the SO_REUSEADDR socket option, allowing the socket to bind to an address that is already in the TIME_WAIT state.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to configure.`

### Methods

#### `function setReuseAddr( I64 fd ) -> Void`

Enable the SO_REUSEADDR socket option, allowing the socket to bind to an address that is already in the TIME_WAIT state.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to configure.`

## function `setReusePort`

Enable the SO_REUSEPORT socket option, allowing multiple sockets to bind to the same port for load balancing across processes.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to configure.`

### Methods

#### `function setReusePort( I64 fd ) -> Void`

Enable the SO_REUSEPORT socket option, allowing multiple sockets to bind to the same port for load balancing across processes.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to configure.`

## function `setTcpNoDelay`

Enable the TCP_NODELAY socket option, disabling Nagle's algorithm to send data immediately without waiting to coalesce small packets.

**Parameters**:

- `fd` (`I64`)
- `The TCP socket file descriptor to configure.`

### Methods

#### `function setTcpNoDelay( I64 fd ) -> Void`

Enable the TCP_NODELAY socket option, disabling Nagle's algorithm to send data immediately without waiting to coalesce small packets.

**Parameters**:

- `fd` (`I64`)
- `The TCP socket file descriptor to configure.`

## function `sendString`

Send an entire string through a connected socket, retrying partial sends until all bytes have been transmitted.

**Parameters**:

- `socketDescriptor` (`I64`)
- `The socket file descriptor to send data on.`
- `content` (`String`)
- `The string to send in its entirety.`

**Returns**: — The total number of bytes sent.

**Raises**:

- `SocketError` → `Error` — If any underlying send operation fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function sendString( I64 socketDescriptor, String content ) -> I64`

Send an entire string through a connected socket, retrying partial sends until all bytes have been transmitted.

**Parameters**:

- `socketDescriptor` (`I64`)
- `The socket file descriptor to send data on.`
- `content` (`String`)
- `The string to send in its entirety.`

**Returns**: — The total number of bytes sent.

**Raises**:

- `SocketError` → `Error` — If any underlying send operation fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `recvString`

Receive data from a connected socket and return it as a null-terminated string. If no data is available or the connection is closed, returns an empty string.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to receive data from.`
- `maxLen` (`I64`)
- `The maximum number of bytes to receive.`

**Returns**: — The received data as a string, or an empty string if no data was received.

**Raises**:

- `SocketError` → `Error` — If the underlying recv operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function recvString( I64 fd, I64 maxLen ) -> String`

Receive data from a connected socket and return it as a null-terminated string. If no data is available or the connection is closed, returns an empty string.

**Parameters**:

- `fd` (`I64`)
- `The socket file descriptor to receive data from.`
- `maxLen` (`I64`)
- `The maximum number of bytes to receive.`

**Returns**: — The received data as a string, or an empty string if no data was received.

**Raises**:

- `SocketError` → `Error` — If the underlying recv operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `createTcpSocket`

Create a new TCP socket using IPv4 (AF_INET, SOCK_STREAM).

**Returns**: — The file descriptor of the newly created TCP socket.

**Raises**:

- `SocketError` → `Error` — If socket creation fails.

### Methods

#### `function createTcpSocket(  ) -> I64`

Create a new TCP socket using IPv4 (AF_INET, SOCK_STREAM).

**Returns**: — The file descriptor of the newly created TCP socket.

**Raises**:

- `SocketError` → `Error` — If socket creation fails.

## function `createUdpSocket`

Create a new UDP socket using IPv4 (AF_INET, SOCK_DGRAM).

**Returns**: — The file descriptor of the newly created UDP socket.

**Raises**:

- `SocketError` → `Error` — If socket creation fails.

### Methods

#### `function createUdpSocket(  ) -> I64`

Create a new UDP socket using IPv4 (AF_INET, SOCK_DGRAM).

**Returns**: — The file descriptor of the newly created UDP socket.

**Raises**:

- `SocketError` → `Error` — If socket creation fails.

## function `tcpListen`

Create a TCP server socket, enable address reuse, bind it to all interfaces on the given port, and start listening for connections.

**Parameters**:

- `port` (`I64`)
- `The port number to listen on.`
- `backlog` (`I64`)
- `The maximum number of pending connections in the listen queue.`

**Returns**: — The file descriptor of the listening server socket.

**Raises**:

- `SocketError` → `Error` — If any step of socket creation, binding, or listening fails.
- `AddressInUseError` → `SocketError` → `Error` — If the port is already in use.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function tcpListen( I64 port, I64 backlog ) -> I64`

Create a TCP server socket, enable address reuse, bind it to all interfaces on the given port, and start listening for connections.

**Parameters**:

- `port` (`I64`)
- `The port number to listen on.`
- `backlog` (`I64`)
- `The maximum number of pending connections in the listen queue.`

**Returns**: — The file descriptor of the listening server socket.

**Raises**:

- `SocketError` → `Error` — If any step of socket creation, binding, or listening fails.
- `AddressInUseError` → `SocketError` → `Error` — If the port is already in use.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `tcpConnect`

Create a TCP socket and connect it to the specified remote address and port.

**Parameters**:

- `ip` (`I64`)
- `The IPv4 address of the remote host as a 32-bit integer in network byte order.`
- `port` (`I64`)
- `The port number of the remote host.`

**Returns**: — The file descriptor of the connected TCP socket.

**Raises**:

- `ConnectionRefusedError` → `ConnectionError` → `SocketError` → `Error` — If the remote host refuses the connection.
- `ConnectionError` → `SocketError` → `Error` — If the connection fails for any other reason.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function tcpConnect( I64 ip, I64 port ) -> I64`

Create a TCP socket and connect it to the specified remote address and port.

**Parameters**:

- `ip` (`I64`)
- `The IPv4 address of the remote host as a 32-bit integer in network byte order.`
- `port` (`I64`)
- `The port number of the remote host.`

**Returns**: — The file descriptor of the connected TCP socket.

**Raises**:

- `ConnectionRefusedError` → `ConnectionError` → `SocketError` → `Error` — If the remote host refuses the connection.
- `ConnectionError` → `SocketError` → `Error` — If the connection fails for any other reason.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

