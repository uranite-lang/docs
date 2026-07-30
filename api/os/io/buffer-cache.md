# uranite.os.io.buffer-cache

## Table of Contents

- [Imports](#imports)
- [const `BUFFER_VALID`](#const-buffer-valid)
- [const `BUFFER_DIRTY`](#const-buffer-dirty)
- [const `BUFFER_LOCKED`](#const-buffer-locked)
- [class `BufferEntry`](#class-bufferentry)
  - [`BufferEntry()`](#BufferEntry)
  - [`isValid()`](#isValid)
  - [`isDirty()`](#isDirty)
  - [`isLocked()`](#isLocked)
  - [`markDirty()`](#markDirty)
  - [`clearDirty()`](#clearDirty)
  - [`markValid()`](#markValid)
  - [`retain()`](#retain)
  - [`release()`](#release)
- [class `BufferCache`](#class-buffercache)
  - [`BufferCache()`](#BufferCache)
  - [`lookup()`](#lookup)
  - [`insert()`](#insert)
  - [`markDirty()`](#markDirty)
  - [`clearDirty()`](#clearDirty)
  - [`isDirty()`](#isDirty)
  - [`release()`](#release)
  - [`invalidate()`](#invalidate)
  - [`getDirtyCount()`](#getDirtyCount)
  - [`getCount()`](#getCount)
  - [`getHits()`](#getHits)
  - [`getMisses()`](#getMisses)
  - [`getHitRate()`](#getHitRate)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## const `BUFFER_VALID`

Bitmask flag indicating that the buffer contains valid data. 

## const `BUFFER_DIRTY`

Bitmask flag indicating that the buffer has been modified and needs write-back. 

## const `BUFFER_LOCKED`

Bitmask flag indicating that the buffer is locked for exclusive access. 

## class `BufferEntry`

Represents a single buffer cache entry that maps a device and block number pair to a cached data buffer. Tracks validity, dirty state, lock status, reference count, and last access time for cache eviction decisions.

### Fields

| Name | Type | Access |
|------|------|--------|
| `deviceId` | `I64` | public |
| `blockNumber` | `I64` | public |
| `flags` | `I64` | public |
| `refCount` | `I64` | public |
| `lastAccess` | `I64` | public |

### Methods

#### `function BufferEntry( self ) -> Void`

Construct a new buffer entry with all fields initialized to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isValid( self ) -> Boolean`

Return True if this buffer entry contains valid cached data (bit 0 of flags). 

#### `function isDirty( self ) -> Boolean`

Return True if this buffer entry has been modified and needs write-back to disk (bit 1 of flags). 

#### `function isLocked( self ) -> Boolean`

Return True if this buffer entry is locked for exclusive access (bit 2 of flags). 

#### `function markDirty( self ) -> Void`

Set the dirty flag on this buffer entry, indicating it needs write-back. 

#### `function clearDirty( self ) -> Void`

Clear the dirty flag on this buffer entry after a successful write-back to disk. 

#### `function markValid( self ) -> Void`

Set the valid flag on this buffer entry, indicating it contains usable cached data. 

#### `function retain( self ) -> Void`

Increment the reference count to indicate that another consumer is using this buffer. 

#### `function release( self ) -> Boolean`

Decrement the reference count and return True if the count has reached zero or below, indicating the buffer is no longer in use and eligible for eviction.

**Returns**: — Boolean:
True if the reference count dropped to zero or below.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `BufferCache`

Fixed-size cache of disk blocks that accelerates I/O by keeping frequently accessed blocks in memory. Each cache slot is keyed by a (deviceId, blockNumber) pair. When the cache is full, the least-recently-used entry with zero references is evicted. Tracks hit and miss counts for cache performance monitoring.

### Fields

| Name | Type | Access |
|------|------|--------|
| `deviceIds` | `Memory<I64>` | protect |
| `blockNumbers` | `Memory<I64>` | protect |
| `flags` | `Memory<I64>` | protect |
| `refCounts` | `Memory<I64>` | protect |
| `accessTimes` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `count` | `I64` | protect |
| `hits` | `I64` | protect |
| `misses` | `I64` | protect |
| `accessCounter` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function BufferCache( self, I64 maxEntries ) -> Void`

Construct a new buffer cache with the given maximum number of cache slots. All slots are initialized as empty.

**Parameters**:

- `maxEntries` (`I64`)
- `The maximum number of block buffers that can be cached simultaneously.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function lookup( self, I64 deviceId, I64 blockNumber ) -> I64`

Search the cache for a block identified by its device and block number. On a hit, the access time is updated, the reference count is incremented, and the hit counter is bumped. On a miss, the miss counter is incremented.

**Parameters**:

- `deviceId` (`I64`)
- `The identifier of the block device that owns this block.`
- `blockNumber` (`I64`)
- `The block number within the device to look up.`

**Returns**: — I64:
The slot index of the cached block on a hit, or -1 on a miss.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function insert( self, I64 deviceId, I64 blockNumber ) -> I64`

Insert a block into the cache. If the block is already cached, returns the existing slot. If the cache is full, evicts the least-recently-used entry with zero references. The operation is protected by a spinlock.

**Parameters**:

- `deviceId` (`I64`)
- `The identifier of the block device that owns this block.`
- `blockNumber` (`I64`)
- `The block number within the device to insert.`

**Returns**: — I64:
The slot index where the block was inserted or already existed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function markDirty( self, I64 slot ) -> Void`

Mark the cache slot as dirty, indicating the cached block has been modified and needs to be written back to the underlying block device.

**Parameters**:

- `slot` (`I64`)
- `The cache slot index to mark as dirty.`

#### `function clearDirty( self, I64 slot ) -> Void`

Clear the dirty flag on a cache slot after a successful write-back to disk.

**Parameters**:

- `slot` (`I64`)
- `The cache slot index to clear the dirty flag on.`

#### `function isDirty( self, I64 slot ) -> Boolean`

Check whether the cache slot is marked as dirty.

**Parameters**:

- `slot` (`I64`)
- `The cache slot index to check.`

**Returns**: `Boolean` — True if the slot is dirty and needs write-back.

#### `function release( self, I64 slot ) -> Void`

Release a reference on a cache slot by decrementing its reference count.

**Parameters**:

- `slot` (`I64`)
- `The cache slot index to release.`

#### `function invalidate( self, I64 slot ) -> Void`

Invalidate a cache slot, removing its entry from the cache entirely. Clears all metadata and decrements the cached block count.

**Parameters**:

- `slot` (`I64`)
- `The cache slot index to invalidate.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getDirtyCount( self ) -> I64`

Count the number of cache entries that are both valid and dirty, which represents the number of blocks that need to be flushed to disk during a sync operation.

**Returns**: — I64:
The number of dirty cache entries.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function lookupUnlocked( self, I64 deviceId, I64 blockNumber ) -> I64`

Search the cache for a block without acquiring the spinlock. This is used internally by insert when the lock is already held.

**Parameters**:

- `deviceId` (`I64`)
- `The identifier of the block device that owns this block.`
- `blockNumber` (`I64`)
- `The block number within the device to look up.`

**Returns**: `I64` — The slot index of the cached block on a hit, or -1 on a miss.

#### `function findFreeSlot( self ) -> I64`

Find the first free (invalid) slot in the cache.

**Returns**: `I64` — The index of a free slot, or -1 if the cache is full.

#### `function evictLru( self ) -> I64`

Evict the least-recently-used cache entry that has a reference count of zero. Scans all slots to find the entry with the oldest access time and no active references, then frees that slot for reuse.

**Returns**: `I64` — The slot index of the evicted entry, now available for reuse.

#### `function getCount( self ) -> I64`

Return the current number of valid entries in the cache. 

#### `function getHits( self ) -> I64`

Return the total number of cache lookup hits since creation. 

#### `function getMisses( self ) -> I64`

Return the total number of cache lookup misses since creation. 

#### `function getHitRate( self ) -> I64`

Calculate and return the cache hit rate as a percentage (0-100). Returns zero if no lookups have been performed.

**Returns**: `I64` — The hit rate percentage.

#### `function destroy( self ) -> Void`

Free all memory buffers allocated by this buffer cache.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

