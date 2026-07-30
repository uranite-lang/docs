# uranite.os.net.ipv4

## Table of Contents

- [Imports](#imports)
- [const `PROTO_ICMP`](#const-proto-icmp)
- [const `PROTO_TCP`](#const-proto-tcp)
- [const `PROTO_UDP`](#const-proto-udp)
- [function `ipAddr`](#function-ipaddr)
  - [`ipAddr()`](#ipAddr)
- [function `ipOctet0`](#function-ipoctet0)
  - [`ipOctet0()`](#ipOctet0)
- [function `ipOctet1`](#function-ipoctet1)
  - [`ipOctet1()`](#ipOctet1)
- [function `ipOctet2`](#function-ipoctet2)
  - [`ipOctet2()`](#ipOctet2)
- [function `ipOctet3`](#function-ipoctet3)
  - [`ipOctet3()`](#ipOctet3)
- [function `isSameSubnet`](#function-issamesubnet)
  - [`isSameSubnet()`](#isSameSubnet)
- [class `Ipv4Header`](#class-ipv4header)
  - [`Ipv4Header()`](#Ipv4Header)
  - [`configure()`](#configure)
  - [`getPayloadLength()`](#getPayloadLength)
  - [`isTcp()`](#isTcp)
  - [`isUdp()`](#isUdp)
  - [`isIcmp()`](#isIcmp)
  - [`dontFragment()`](#dontFragment)
  - [`moreFragments()`](#moreFragments)
  - [`setDontFragment()`](#setDontFragment)
  - [`decrementTtl()`](#decrementTtl)
- [class `RoutingTable`](#class-routingtable)
  - [`RoutingTable()`](#RoutingTable)
  - [`addRoute()`](#addRoute)
  - [`removeRoute()`](#removeRoute)
  - [`lookup()`](#lookup)
  - [`lookupInterface()`](#lookupInterface)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## const `PROTO_ICMP`

IP protocol number for ICMP (Internet Control Message Protocol). ICMP is used for diagnostic and error reporting functions such as ping and traceroute.

## const `PROTO_TCP`

IP protocol number for TCP (Transmission Control Protocol). TCP provides reliable, ordered, connection-oriented byte stream delivery over IP.

## const `PROTO_UDP`

IP protocol number for UDP (User Datagram Protocol). UDP provides connectionless, unreliable datagram delivery with minimal overhead.

## function `ipAddr`

Constructs an IPv4 address from four individual octets by packing them into a single I64 value in network byte order. For example, ipAddr(192, 168, 1, 1) produces the 32-bit representation of 192.168.1.1.

### Methods

#### `function ipAddr( I64 octet0, I64 octet1, I64 octet2, I64 octet3 ) -> I64`

Constructs an IPv4 address from four individual octets by packing them into a single I64 value in network byte order. For example, ipAddr(192, 168, 1, 1) produces the 32-bit representation of 192.168.1.1.

## function `ipOctet0`

Extracts the first (most significant) octet from an IPv4 address stored as an I64 value. For address 192.168.1.1, this returns 192.

### Methods

#### `function ipOctet0( I64 ip ) -> I64`

Extracts the first (most significant) octet from an IPv4 address stored as an I64 value. For address 192.168.1.1, this returns 192.

## function `ipOctet1`

Extracts the second octet from an IPv4 address stored as an I64 value. For address 192.168.1.1, this returns 168.

### Methods

#### `function ipOctet1( I64 ip ) -> I64`

Extracts the second octet from an IPv4 address stored as an I64 value. For address 192.168.1.1, this returns 168.

## function `ipOctet2`

Extracts the third octet from an IPv4 address stored as an I64 value. For address 192.168.1.1, this returns 1.

### Methods

#### `function ipOctet2( I64 ip ) -> I64`

Extracts the third octet from an IPv4 address stored as an I64 value. For address 192.168.1.1, this returns 1.

## function `ipOctet3`

Extracts the fourth (least significant) octet from an IPv4 address stored as an I64 value. For address 192.168.1.1, this returns 1.

### Methods

#### `function ipOctet3( I64 ip ) -> I64`

Extracts the fourth (least significant) octet from an IPv4 address stored as an I64 value. For address 192.168.1.1, this returns 1.

## function `isSameSubnet`

Determines whether two IPv4 addresses belong to the same subnet by applying the given subnet mask to both addresses and comparing the network portions. Returns True if both addresses share the same network prefix under the mask.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function isSameSubnet( I64 addr1, I64 addr2, I64 mask ) -> Boolean`

Determines whether two IPv4 addresses belong to the same subnet by applying the given subnet mask to both addresses and comparing the network portions. Returns True if both addresses share the same network prefix under the mask.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Ipv4Header`

Represents an IPv4 packet header as defined in RFC 791. The minimum header size is 20 bytes (header length field value of 5, meaning 5 x 4 = 20 bytes) with no options. Fields include version, header length, total packet length, identification for fragmentation reassembly, flags, fragment offset, time-to-live hop counter, protocol identifier, header checksum, and source/destination addresses.

### Fields

| Name | Type | Access |
|------|------|--------|
| `version` | `I64` | public |
| `headerLength` | `I64` | public |
| `totalLength` | `I64` | public |
| `identification` | `I64` | public |
| `flags` | `I64` | public |
| `fragmentOffset` | `I64` | public |
| `ttl` | `I64` | public |
| `protocol` | `I64` | public |
| `checksum` | `I64` | public |
| `srcAddr` | `I64` | public |
| `dstAddr` | `I64` | public |

### Methods

#### `function Ipv4Header( self ) -> Void`

Constructs a new IPv4 header with default values: version 4, header length 5 (20 bytes), total length 20, TTL of 64 hops, and all other fields set to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function configure( self, I64 src, I64 dst, I64 proto, I64 payloadLen ) -> Void`

Configures this IPv4 header with the specified source address, destination address, transport protocol number, and payload length. The total length field is automatically set to 20 (header size) plus the payload length.

#### `function getPayloadLength( self ) -> I64`

Calculates and returns the payload length in bytes by subtracting the header size (header length multiplied by 4) from the total packet length.

#### `function isTcp( self ) -> Boolean`

Returns True if the protocol field indicates TCP (protocol number 6). 

#### `function isUdp( self ) -> Boolean`

Returns True if the protocol field indicates UDP (protocol number 17). 

#### `function isIcmp( self ) -> Boolean`

Returns True if the protocol field indicates ICMP (protocol number 1). 

#### `function dontFragment( self ) -> Boolean`

Returns True if the Don't Fragment (DF) flag is set. The DF flag is bit 1 of the 3-bit flags field. When set, routers must not fragment this packet and should return an ICMP Fragmentation Needed error instead.

#### `function moreFragments( self ) -> Boolean`

Returns True if the More Fragments (MF) flag is set. The MF flag is bit 2 of the 3-bit flags field. When set, it indicates that more fragments follow this one as part of a fragmented datagram.

#### `function setDontFragment( self ) -> Void`

Sets the Don't Fragment (DF) flag in the flags field, instructing routers along the path not to fragment this packet.

#### `function decrementTtl( self ) -> Boolean`

Decrements the time-to-live (TTL) field by one, as performed by each router that forwards the packet. Returns True if the TTL was successfully decremented (TTL was greater than 1), or False if the TTL has expired (TTL was 1 or less), indicating the packet should be discarded and an ICMP Time Exceeded message sent.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `RoutingTable`

IPv4 routing table that performs longest-prefix-match lookups to determine the next-hop gateway and outgoing interface for a given destination address. Each route entry consists of a network address, subnet mask, gateway address, interface identifier, and metric value for tie-breaking when multiple routes match. Routes are stored in parallel arrays for cache-friendly linear scanning.

### Fields

| Name | Type | Access |
|------|------|--------|
| `networks` | `Memory<I64>` | public |
| `masks` | `Memory<I64>` | public |
| `gateways` | `Memory<I64>` | public |
| `interfaces` | `Memory<I64>` | public |
| `metrics` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `count` | `I64` | public |

### Methods

#### `function RoutingTable( self, I64 maxRoutes ) -> Void`

Constructs a new routing table with the specified maximum number of route entries. All slots are initialized to zero.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function addRoute( self, I64 network, I64 mask, I64 gateway, I64 iface, I64 metric ) -> Boolean`

Adds a new route to the routing table with the specified network address, subnet mask, gateway address, outgoing interface identifier, and routing metric. Returns True if the route was successfully added, or False if the table is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function removeRoute( self, I64 network, I64 mask ) -> Boolean`

Removes a route from the routing table that matches the given network address and subnet mask. The last entry in the table is moved into the vacated slot to maintain a compact array. Returns True if the route was found and removed, or False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function lookup( self, I64 destAddr ) -> I64`

Performs a longest-prefix-match lookup for the given destination IPv4 address. Iterates through all routes and selects the one with the longest matching prefix (most specific subnet mask). When multiple routes have the same prefix length, the route with the lowest metric is preferred. Returns the gateway address of the best matching route, or 0 if no route matches.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function lookupInterface( self, I64 destAddr ) -> I64`

Performs a longest-prefix-match lookup for the given destination IPv4 address and returns the outgoing interface identifier of the best matching route. Returns -1 (encoded as 0 - 1) if no route matches the destination address.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function countBits( self, I64 mask ) -> I64`

Counts the number of set bits (population count) in the given subnet mask value. This determines the prefix length of the subnet, used for longest-prefix-match comparison during route lookups.

#### `function getCount( self ) -> I64`

Returns the number of routes currently stored in the routing table. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the routing table, including the networks, masks, gateways, interfaces, and metrics arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

