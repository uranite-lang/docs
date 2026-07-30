# uranite.os.memory.heap

## Table of Contents

- [Imports](#imports)
- [class `SlabCache`](#class-slabcache)
  - [`SlabCache()`](#SlabCache)
  - [`allocate()`](#allocate)
  - [`deallocate()`](#deallocate)
  - [`getObjectSize()`](#getObjectSize)
  - [`getUsed()`](#getUsed)
  - [`getCapacity()`](#getCapacity)
  - [`isFull()`](#isFull)
  - [`destroy()`](#destroy)
- [class `KernelHeap`](#class-kernelheap)
  - [`KernelHeap()`](#KernelHeap)
  - [`init()`](#init)
  - [`kmalloc()`](#kmalloc)
  - [`kfree()`](#kfree)
  - [`isInitialized()`](#isInitialized)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.memory.pmm`
  - `PhysicalMemoryManager`

## class `SlabCache`

Fixed-size object cache backed by 4KB pages from the Physical Memory Manager. Unused slots are chained into a free list, providing O(1) allocation and deallocation. Each cache handles objects of a single size, and multiple caches are combined by KernelHeap to serve various allocation sizes.

### Fields

| Name | Type | Access |
|------|------|--------|
| `objectSize` | `I64` | protect |
| `backing` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `used` | `I64` | protect |
| `firstFree` | `I64` | protect |

### Methods

#### `function SlabCache( self, I64 objectSize, I64 pageCount ) -> Void`

Construct a slab cache for objects of the given size, backed by the specified number of 4KB pages. Computes the total capacity from the available bytes and initializes a free list where each slot points to the next free slot, with the last slot pointing to -1.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function allocate( self ) -> I64`

Allocate one object slot from the cache by popping the head of the free list. Returns the slot index of the allocated object, or -1 if the cache is full and no free slots remain.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function deallocate( self, I64 slot ) -> Void`

Return an object slot back to the cache by pushing it onto the head of the free list. The slot index must be valid and within the capacity of the cache.

#### `function getObjectSize( self ) -> I64`

Return the fixed object size in bytes that this cache handles. 

#### `function getUsed( self ) -> I64`

Return the number of currently allocated object slots. 

#### `function getCapacity( self ) -> I64`

Return the total number of object slots available in this cache. 

#### `function isFull( self ) -> Boolean`

Return whether all object slots in this cache are currently allocated. 

#### `function destroy( self ) -> Void`

Free the backing memory used for the slab cache storage.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `KernelHeap`

Kernel heap allocator that routes allocation requests to power-of-2 slab caches (16, 32, 64, 128, 256, 512, 1024, 2048 bytes). Small allocations below 4096 bytes are served by the slab cache matching the rounded-up size. Large allocations of 4096 bytes or more bypass the caches entirely and allocate contiguous pages directly from the Physical Memory Manager.

### Fields

| Name | Type | Access |
|------|------|--------|
| `cache16` | `SlabCache` | protect |
| `cache32` | `SlabCache` | protect |
| `cache64` | `SlabCache` | protect |
| `cache128` | `SlabCache` | protect |
| `cache256` | `SlabCache` | protect |
| `cache512` | `SlabCache` | protect |
| `cache1024` | `SlabCache` | protect |
| `cache2048` | `SlabCache` | protect |
| `pmm` | `PhysicalMemoryManager` | protect |
| `initialized` | `Boolean` | protect |

### Methods

#### `function KernelHeap( self, PhysicalMemoryManager pmm ) -> Void`

Construct a kernel heap backed by the given Physical Memory Manager. The heap is not usable until init() is called to create the slab caches.

#### `function init( self ) -> Void`

Initialize all slab caches with 4 pages (16KB) each. This must be called after the Physical Memory Manager and Virtual Memory Manager are fully initialized and ready to serve allocation requests.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function kmalloc( self, I64 size ) -> I64`

Allocate memory of the given size in bytes. Routes the request to the smallest slab cache that can fit the allocation. For sizes exceeding 2048 bytes, rounds up to the nearest page boundary and allocates contiguous physical frames directly from the PMM. Returns a slot index for slab allocations, or a physical frame address for large ones.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function kfree( self, I64 ptr, I64 size ) -> Void`

Free previously allocated memory. The original allocation size must be provided so the heap can route the deallocation to the correct slab cache. For large allocations exceeding 2048 bytes, the corresponding physical frames are returned to the PMM. The size must match the original kmalloc request, or be tracked externally by the caller.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function isInitialized( self ) -> Boolean`

Return whether the kernel heap has been initialized and is ready to serve allocations.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

