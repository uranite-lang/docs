# uranite.os.net.ethernet

## Table of Contents

- [Imports](#imports)
- [const `ETHERTYPE_IPV4`](#const-ethertype-ipv4)
- [const `ETHERTYPE_ARP`](#const-ethertype-arp)
- [const `ETHERTYPE_IPV6`](#const-ethertype-ipv6)
- [const `ETH_MTU`](#const-eth-mtu)
- [const `ETH_MIN_PAYLOAD`](#const-eth-min-payload)
- [class `EthernetFrame`](#class-ethernetframe)
  - [`EthernetFrame()`](#EthernetFrame)
  - [`configure()`](#configure)
  - [`isBroadcast()`](#isBroadcast)
  - [`isMulticast()`](#isMulticast)
  - [`isIpv4()`](#isIpv4)
  - [`isArp()`](#isArp)
  - [`isIpv6()`](#isIpv6)
  - [`getTotalSize()`](#getTotalSize)
- [class `ArpTable`](#class-arptable)
  - [`ArpTable()`](#ArpTable)
  - [`update()`](#update)
  - [`lookup()`](#lookup)
  - [`remove()`](#remove)
  - [`contains()`](#contains)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## const `ETHERTYPE_IPV4`

EtherType constant for IPv4 (0x0800). The EtherType field in an Ethernet frame header identifies the protocol encapsulated in the payload.

## const `ETHERTYPE_ARP`

EtherType constant for ARP (0x0806). Address Resolution Protocol frames use this EtherType to resolve IPv4 addresses to MAC addresses on the local network segment.

## const `ETHERTYPE_IPV6`

EtherType constant for IPv6 (0x86DD). IPv6 frames use this EtherType value in the Ethernet header to indicate an IPv6 payload.

## const `ETH_MTU`

Maximum transmission unit for Ethernet payload data (1500 bytes). The largest payload size that can be carried in a standard Ethernet frame without fragmentation.

## const `ETH_MIN_PAYLOAD`

Minimum Ethernet frame payload size (46 bytes). Frames with payloads shorter than this must be padded to meet the minimum frame size required by the Ethernet specification (64 bytes total including header and FCS).

## class `EthernetFrame`

Represents an Ethernet Layer 2 frame header. The Ethernet frame format consists of a 6-byte destination MAC address, 6-byte source MAC address, 2-byte EtherType field, and 46-1500 bytes of payload followed by a 4-byte frame check sequence (FCS). MAC addresses are stored as I64 values with only the lower 48 bits used.

### Fields

| Name | Type | Access |
|------|------|--------|
| `destMac` | `I64` | public |
| `srcMac` | `I64` | public |
| `ethertype` | `I64` | public |
| `payloadLength` | `I64` | public |

### Methods

#### `function EthernetFrame( self ) -> Void`

Constructs a new EthernetFrame with all fields initialized to zero, representing an empty frame that must be configured before use.

#### `function configure( self, I64 dst, I64 src, I64 ethType, I64 len ) -> Void`

Configures this Ethernet frame header with the specified destination MAC address, source MAC address, EtherType protocol identifier, and payload length in bytes.

#### `function isBroadcast( self ) -> Boolean`

Returns True if the destination MAC address is the broadcast address (FF:FF:FF:FF:FF:FF = 281474976710655 decimal). Broadcast frames are delivered to all hosts on the local network segment.

#### `function isMulticast( self ) -> Boolean`

Returns True if the destination MAC address has the multicast bit set (bit 40, the least significant bit of the first octet). Multicast frames are delivered to a group of hosts that have subscribed to the multicast address.

#### `function isIpv4( self ) -> Boolean`

Returns True if this frame carries an IPv4 payload, indicated by an EtherType value of 0x0800 (2048 decimal).

#### `function isArp( self ) -> Boolean`

Returns True if this frame carries an ARP payload, indicated by an EtherType value of 0x0806 (2054 decimal).

#### `function isIpv6( self ) -> Boolean`

Returns True if this frame carries an IPv6 payload, indicated by an EtherType value of 0x86DD (34525 decimal).

#### `function getTotalSize( self ) -> I64`

Returns the total frame size in bytes, calculated as the 14-byte header (6 bytes destination MAC + 6 bytes source MAC + 2 bytes EtherType) plus the payload length. Does not include the 4-byte FCS.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ArpTable`

Address Resolution Protocol table that maps IPv4 addresses to MAC (hardware) addresses. The ARP table is used by the network stack to resolve Layer 3 addresses to Layer 2 addresses for local network communication. Each entry has an associated state value where 1 indicates a resolved entry and 0 indicates an empty slot. The table uses a spinlock for thread-safe concurrent access from interrupt handlers and kernel threads.

### Fields

| Name | Type | Access |
|------|------|--------|
| `ipAddresses` | `Memory<I64>` | public |
| `macAddresses` | `Memory<I64>` | public |
| `timestamps` | `Memory<I64>` | public |
| `states` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `count` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function ArpTable( self, I64 maxEntries ) -> Void`

Constructs a new ARP table with the specified maximum number of entries. All slots are initialized to empty (state 0). A spinlock is created for concurrent access protection.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function update( self, I64 ip, I64 mac, I64 timestamp ) -> Boolean`

Adds or updates an ARP entry for the given IPv4 address with the specified MAC address and timestamp. If the IP already exists in the table, the MAC and timestamp are updated in place and the state is set to resolved (1). If the IP is not found, a new entry is created in the first available empty slot. Returns True on success or False if the table is full and the entry does not already exist.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function lookup( self, I64 ip ) -> I64`

Looks up the MAC address associated with the given IPv4 address in the ARP table. Returns the MAC address as an I64 if a resolved entry exists for the IP, or 0 if no matching entry is found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function remove( self, I64 ip ) -> Boolean`

Removes the ARP table entry for the given IPv4 address. Clears the IP, MAC, timestamp, and state fields for the matching slot and decrements the entry count. Returns True if the entry was found and removed, or False if no matching entry exists.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function contains( self, I64 ip ) -> Boolean`

Checks whether the ARP table contains a resolved entry for the given IPv4 address. Returns True if a resolved entry (state 1) exists for the IP, or False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Returns the number of active entries currently stored in the ARP table. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by this ARP table, including the IP address, MAC address, timestamp, and state arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

