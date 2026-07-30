# uranite.net.protocol

## Table of Contents

- [Imports](#imports)
- [class `NetworkStack`](#class-networkstack)
  - [`NetworkStack()`](#NetworkStack)
  - [`resolveAddress()`](#resolveAddress)
  - [`routePacket()`](#routePacket)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.net.ethernet`
  - `ArpTable`
  - `ETHERTYPE_ARP`
  - `ETHERTYPE_IPV4`
  - `ETHERTYPE_IPV6`
  - `ETH_MIN_PAYLOAD`
  - `ETH_MTU`
  - `EthernetFrame`
- `uranite.os.net.ipv4`
  - `Ipv4Header`
  - `PROTO_ICMP`
  - `PROTO_TCP`
  - `PROTO_UDP`
  - `RoutingTable`
  - `ipAddr`
  - `ipOctet0`
  - `ipOctet1`
  - `ipOctet2`
  - `ipOctet3`
  - `isSameSubnet`
- `uranite.os.net.socket`
  - `AF_INET`
  - `AF_INET6`
  - `SOCKET_BOUND`
  - `SOCKET_CLOSED`
  - `SOCKET_CONNECTED`
  - `SOCKET_DATAGRAM`
  - `SOCKET_LISTENING`
  - `SOCKET_RAW`
  - `SOCKET_STREAM`
  - `SOCKET_UNBOUND`
  - `SocketTable`
- `uranite.os.net.tcp`
  - `TCP_ACK`
  - `TCP_FIN`
  - `TCP_HEADER_SIZE`
  - `TCP_PSH`
  - `TCP_RST`
  - `TCP_SYN`
  - `TCP_URG`
  - `TcpConnectionTable`
  - `TcpHeader`
  - `TcpState`
- `uranite.os.net.udp`
  - `UDP_HEADER_SIZE`
  - `UdpBindTable`
  - `UdpHeader`

## class `NetworkStack`

Full network protocol stack: Ethernet (L2) -> IPv4 (L3) -> TCP/UDP (L4) with socket layer. Combines ARP resolution, IP routing, TCP connection tracking, UDP port binding, and socket lifecycle management into a single unified interface for kernel networking.

### Fields

| Name | Type | Access |
|------|------|--------|
| `arpTable` | `ArpTable` | public |
| `routingTable` | `RoutingTable` | public |
| `tcpConnections` | `TcpConnectionTable` | public |
| `udpBindings` | `UdpBindTable` | public |
| `sockets` | `SocketTable` | public |

### Methods

#### `function NetworkStack( self, I64 arpCapacity, I64 routeCapacity, I64 tcpCapacity, I64 udpCapacity, I64 socketCapacity ) -> Void`

Construct a network stack with specified capacities for each subsystem.

**Parameters**:

- `arpCapacity` (`I64`)
- `Maximum ARP table entries.`
- `routeCapacity` (`I64`)
- `Maximum routing table entries.`
- `tcpCapacity` (`I64`)
- `Maximum concurrent TCP connections.`
- `udpCapacity` (`I64`)
- `Maximum UDP port bindings.`
- `socketCapacity` (`I64`)
- `Maximum open sockets.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function resolveAddress( self, I64 ipAddress ) -> I64`

Resolve an IPv4 address to a MAC address via ARP table lookup. Returns the MAC address or 0 if not found.

**Parameters**:

- `ipAddress` (`I64`)
- `IPv4 address to resolve.`

#### `function routePacket( self, I64 destination ) -> I64`

Determine the next-hop gateway for a destination via longest-prefix-match. Returns the gateway address or 0 if no route matches.

**Parameters**:

- `destination` (`I64`)
- `Destination IPv4 address.`

#### `function destroy( self ) -> Void`

Release all resources held by every subsystem.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

