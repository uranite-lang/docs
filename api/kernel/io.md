# uranite.kernel.io

## Table of Contents

- [Imports](#imports)
- [class `BlockIoSubsystem`](#class-blockiosubsystem)
  - [`BlockIoSubsystem()`](#BlockIoSubsystem)
  - [`lookupCached()`](#lookupCached)
  - [`submitRead()`](#submitRead)
  - [`submitWrite()`](#submitWrite)
  - [`getDirtyCount()`](#getDirtyCount)
  - [`getCacheHits()`](#getCacheHits)
  - [`getPendingRequests()`](#getPendingRequests)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.io.buffer-cache`
  - `BUFFER_DIRTY`
  - `BUFFER_LOCKED`
  - `BUFFER_VALID`
  - `BufferCache`
  - `BufferEntry`
- `uranite.os.io.dma`
  - `DmaChannel`
  - `DmaDescriptor`
  - `DmaDirection`
- `uranite.os.io.request`
  - `IoRequestQueue`
  - `RequestStatus`
  - `RequestType`

## class `BlockIoSubsystem`

Unified block I/O subsystem combining buffer cache for read caching, DMA channels for direct memory transfers, and request queues for asynchronous I/O scheduling.

### Fields

| Name | Type | Access |
|------|------|--------|
| `cache` | `BufferCache` | public |
| `requestQueue` | `IoRequestQueue` | public |

### Methods

#### `function BlockIoSubsystem( self, I64 cacheCapacity, I64 queueCapacity ) -> Void`

#### `function lookupCached( self, I64 deviceId, I64 blockNumber ) -> I64`

Look up a block in the buffer cache. Returns the cache slot index or -1 if not cached.

#### `function submitRead( self, I64 deviceId, I64 blockNumber, I64 bufferAddress, I64 length, I64 tag ) -> I64`

Submit an asynchronous read request. Returns the request index for later status checking.

#### `function submitWrite( self, I64 deviceId, I64 blockNumber, I64 bufferAddress, I64 length, I64 tag ) -> I64`

Submit an asynchronous write request. Returns the request index for later status checking.

#### `function getDirtyCount( self ) -> I64`

Return the number of dirty buffer cache entries awaiting writeback. 

#### `function getCacheHits( self ) -> I64`

Return the number of cache hits since creation. 

#### `function getPendingRequests( self ) -> I64`

Return the number of pending I/O requests. 

#### `function destroy( self ) -> Void`

Release all resources held by the block I/O subsystem.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

