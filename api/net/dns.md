# uranite.net.dns

## Table of Contents

- [Imports](#imports)
- [const `DNS_PORT`](#const-dns-port)
- [const `DNS_HEADER_SIZE`](#const-dns-header-size)
- [const `DNS_MAX_RESPONSE_SIZE`](#const-dns-max-response-size)
- [const `DNS_QUERY_ID`](#const-dns-query-id)
- [const `DNS_FLAGS_STANDARD_QUERY`](#const-dns-flags-standard-query)
- [const `DNS_QTYPE_A`](#const-dns-qtype-a)
- [const `DNS_QCLASS_IN`](#const-dns-qclass-in)
- [const `DNS_LABEL_COMPRESSION_MASK`](#const-dns-label-compression-mask)
- [const `DNS_A_RECORD_RDLENGTH`](#const-dns-a-record-rdlength)
- [class `DnsResolver`](#class-dnsresolver)
  - [`DnsResolver()`](#DnsResolver)
  - [`readNameserver()`](#readNameserver)
  - [`resolve()`](#resolve)
  - [`resolveWith()`](#resolveWith)
  - [`parseResponse()`](#parseResponse)
  - [`skipName()`](#skipName)

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
- `uranite.net.socket`
  - `AF_INET`
  - `IPPROTO_UDP`
  - `SOCK_DGRAM`
  - `SYS_RECVFROM`
  - `SYS_SENDTO`
  - `buildSockaddrIn`
  - `ipToInt`
  - `socketClose`
  - `socketCreate`
- `uranite.os.syscall.invoke`
  - `syscall1`
  - `syscall3`
  - `syscall6`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `DNS_PORT`

Standard DNS service port number.

## const `DNS_HEADER_SIZE`

Fixed size in bytes of a DNS message header.

## const `DNS_MAX_RESPONSE_SIZE`

Maximum size in bytes of a standard DNS UDP response (RFC 1035).

## const `DNS_QUERY_ID`

Fixed transaction ID used for DNS queries.

## const `DNS_FLAGS_STANDARD_QUERY`

DNS header flags for a standard recursive query (QR=0, RD=1).

## const `DNS_QTYPE_A`

DNS query type for IPv4 address (A record).

## const `DNS_QCLASS_IN`

DNS query class for Internet addresses.

## const `DNS_LABEL_COMPRESSION_MASK`

Bitmask identifying a DNS label compression pointer (0xC0).

## const `DNS_A_RECORD_RDLENGTH`

Expected RDATA length in bytes for an A record (IPv4 = 4 bytes).

## class `DnsResolver`

Stateless DNS stub resolver providing hostname-to-IPv4 resolution via standard DNS A-record queries over UDP. All methods are static and operate without instance state — the class serves purely as a namespace for DNS resolution operations.

The resolver reads /etc/resolv.conf for the system nameserver address, constructs RFC 1035 compliant query packets, sends them via UDP to port 53, and parses the response for the first A-record answer.

### Methods

#### `function DnsResolver( self ) -> Void`

No-op constructor — all methods are static.

#### `function readNameserver(  ) -> I64`

`static` 

Parse /etc/resolv.conf to extract the first nameserver IPv4 address. Scans each line for the "nameserver" directive followed by a dotted-decimal IPv4 address, ignoring comments and IPv6 entries.

**Returns**: — I64:
Packed IPv4 address of the first nameserver found, or
the loopback address 127.0.0.1 (0x7F000001) if the file
is unreadable or contains no valid nameserver entries.

**Complexity**:
- Time: `O(f) where f is file size`
- Space: `O(f)`

#### `function resolve( String hostname ) -> I64`

`static` 

Resolve a hostname to an IPv4 address via DNS over UDP. Reads the system nameserver from /etc/resolv.conf, constructs a standard DNS A-record query, sends it to the nameserver on port 53, and parses the first A-record from the response.

**Parameters**:

- `hostname` (`String`)
- `The domain name to resolve` (`e.g., "www.google.com"`)

**Returns**: — I64:
Packed IPv4 address from the first A-record in the response,
or -1 if the query fails or no A-record is found.

**Complexity**:
- Time: `O(n) where n is hostname length + response size`
- Space: `O(n)`

#### `function resolveWith( String hostname, I64 nameserverIp ) -> I64`

`static` 

Resolve a hostname to an IPv4 address via DNS over UDP using a specific nameserver. Constructs an RFC 1035 compliant A-record query, sends it via UDP, and parses the first A-record answer.

**Parameters**:

- `hostname` (`String`)
- `The domain name to resolve` (`e.g., "www.google.com"`)
- `nameserverIp` (`I64`)
- `Packed IPv4 address of the DNS nameserver to query.`

**Returns**: — I64:
Packed IPv4 address from the first A-record in the response,
or -1 if the query fails or no A-record is found.

**Complexity**:
- Time: `O(n) where n is hostname length + response size`
- Space: `O(n)`

#### `function parseResponse( I64 responseBuffer, I64 responseSize ) -> I64`

`static` 

Parse a DNS response packet and extract the first A-record IPv4 address. Validates the answer count, skips the question section, then iterates answer resource records looking for type A with 4-byte RDATA.

**Parameters**:

- `responseBuffer` (`I64`)
- `Memory address of the raw DNS response packet.`
- `responseSize` (`I64`)
- `Total size in bytes of the response packet.`

**Returns**: — I64:
Packed IPv4 address from the first A-record, or -1 if
no valid A-record is found in the response.

**Complexity**:
- Time: `O(r) where r is response size`
- Space: `O(1)`

#### `function skipName( I64 buffer, I64 bufferSize, I64 offset ) -> I64`

`static` 

Advance past a DNS domain name in a packet, handling both inline labels and compression pointers (RFC 1035 Section 4.1.4).

**Parameters**:

- `buffer` (`I64`)
- `Memory address of the DNS packet.`
- `bufferSize` (`I64`)
- `Total size in bytes of the packet.`
- `offset` (`I64`)
- `Current byte offset pointing to the start of the name.`

**Returns**: — I64:
Byte offset immediately after the name field.

**Complexity**:
- Time: `O(n) where n is encoded name length`
- Space: `O(1)`

