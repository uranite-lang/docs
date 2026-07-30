# uranite.os.ipc.shared-memory

## Table of Contents

- [Imports](#imports)
- [class `SharedRegion`](#class-sharedregion)
  - [`SharedRegion()`](#SharedRegion)
  - [`getRegionId()`](#getRegionId)
  - [`getPhysicalBase()`](#getPhysicalBase)
  - [`getSize()`](#getSize)
  - [`getPageCount()`](#getPageCount)
  - [`getOwnerTaskId()`](#getOwnerTaskId)
  - [`getFlags()`](#getFlags)
  - [`retain()`](#retain)
  - [`release()`](#release)
  - [`getRefCount()`](#getRefCount)
  - [`isWritable()`](#isWritable)
  - [`isExecutable()`](#isExecutable)
- [class `SharedMemoryRegistry`](#class-sharedmemoryregistry)
  - [`SharedMemoryRegistry()`](#SharedMemoryRegistry)
  - [`allocateRegionId()`](#allocateRegionId)
  - [`addRegion()`](#addRegion)
  - [`findRegion()`](#findRegion)
  - [`removeRegion()`](#removeRegion)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `SharedRegion`

Represents a shared memory region that maps the same physical page frames into multiple address spaces for zero-copy IPC. This is the fastest IPC path available. The caller is responsible for synchronization using Spinlock or Mutex. Reference counting tracks how many address spaces currently map the region.

### Fields

| Name | Type | Access |
|------|------|--------|
| `regionId` | `I64` | protect |
| `physicalBase` | `I64` | protect |
| `pageCount` | `I64` | protect |
| `ownerTaskId` | `I64` | protect |
| `refCount` | `I64` | protect |
| `flags` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function SharedRegion( self, I64 id, I64 owner, I64 physBase, I64 pages, I64 regionFlags ) -> Void`

Construct a new shared memory region backed by contiguous physical pages.

**Parameters**:

- `id` (`I64`)
- `The unique region identifier.`
- `owner` (`I64`)
- `The task identifier of the region's creator.`
- `physBase` (`I64`)
- `The physical base address of the memory region.`
- `pages` (`I64`)
- `The number of 4096-byte pages in the region.`
- `regionFlags` (`I64`)
- `Permission flags` (`bit 0 = readable, bit 1 = writable, bit 2 = executable`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getRegionId( self ) -> I64`

Return the unique identifier of this shared memory region. 

#### `function getPhysicalBase( self ) -> I64`

Return the physical base address of the underlying memory. 

#### `function getSize( self ) -> I64`

Return the total size of the shared region in bytes (page count multiplied by 4096). 

#### `function getPageCount( self ) -> I64`

Return the number of 4096-byte pages in this shared region. 

#### `function getOwnerTaskId( self ) -> I64`

Return the task identifier of the task that created this shared region. 

#### `function getFlags( self ) -> I64`

Return the permission flags bitmask for this shared region. 

#### `function retain( self ) -> I64`

Increment the reference count when mapping this region into another address space. Protected by a spinlock.

**Returns**: — I64:
The new reference count after incrementing.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function release( self ) -> Boolean`

Decrement the reference count when unmapping this region from an address space. Protected by a spinlock.

**Returns**: — Boolean:
True when the last reference has been released and the physical
pages can be freed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getRefCount( self ) -> I64`

Return the current number of address spaces mapping this shared region. 

#### `function isWritable( self ) -> Boolean`

Return True if this shared region permits write access (bit 1 of flags). 

#### `function isExecutable( self ) -> Boolean`

Return True if this shared region permits code execution (bit 2 of flags).

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `SharedMemoryRegistry`

Registry of all active shared memory regions in the kernel. Tracks regions by identifier for lookup during map and unmap syscalls. Uses a fixed-capacity array with linear search and spinlock protection for concurrent access.

### Fields

| Name | Type | Access |
|------|------|--------|
| `regionPointers` | `Memory<I64>` | protect |
| `regionIds` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `count` | `I64` | protect |
| `nextRegionId` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function SharedMemoryRegistry( self, I64 maxRegions ) -> Void`

Construct a new shared memory registry with the given maximum capacity.

**Parameters**:

- `maxRegions` (`I64`)
- `The maximum number of shared memory regions that can be tracked.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function allocateRegionId( self ) -> I64`

Allocate the next unique region identifier from a monotonically increasing counter.

**Returns**: `I64` — A unique region identifier.

#### `function addRegion( self, I64 regionId, I64 regionPointer ) -> I64`

Register a shared memory region in the registry. Searches for the first free slot and stores the region identifier and pointer. Protected by a spinlock.

**Parameters**:

- `regionId` (`I64`)
- `The unique identifier of the region to register.`
- `regionPointer` (`I64`)
- `The type-erased pointer to the SharedRegion object.`

**Returns**: — I64:
The slot index where the region was stored, or -1 if the registry is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findRegion( self, I64 regionId ) -> I64`

Find a shared memory region by its identifier.

**Parameters**:

- `regionId` (`I64`)
- `The unique identifier of the region to find.`

**Returns**: — I64:
The type-erased pointer to the SharedRegion object, or 0 if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function removeRegion( self, I64 regionId ) -> Boolean`

Remove a shared memory region from the registry by its identifier. Protected by a spinlock.

**Parameters**:

- `regionId` (`I64`)
- `The unique identifier of the region to remove.`

**Returns**: — Boolean:
True if the region was found and removed, False if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Return the number of shared memory regions currently registered. 

#### `function destroy( self ) -> Void`

Free the memory arrays allocated by this registry.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

