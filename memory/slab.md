# uranite.memory.slab

## Table of Contents

- [Imports](#imports)
- [class `SlabAllocator`](#class-slaballocator)
  - [`SlabAllocator()`](#SlabAllocator)
  - [`allocate()`](#allocate)
  - [`deallocate()`](#deallocate)
  - [`getObjectSize()`](#getObjectSize)
  - [`getUsed()`](#getUsed)
  - [`getCapacity()`](#getCapacity)
  - [`isFull()`](#isFull)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.memory.heap`
  - `KernelHeap`
  - `SlabCache`

## class `SlabAllocator`

Slab-based memory allocator for fixed-size object pools. Provides O(1) allocation and deallocation via a free-list backed slab cache. Each allocator serves objects of a single size.

### Fields

| Name | Type | Access |
|------|------|--------|
| `cache` | `SlabCache` | protect |

### Methods

#### `function SlabAllocator( self, I64 objectSize, I64 pageCount ) -> Void`

Construct a slab allocator for objects of the given size.

**Parameters**:

- `objectSize` (`I64`)
- `Size in bytes of each object this allocator serves.`
- `pageCount` (`I64`)
- `Number of 4KB pages backing this cache.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function allocate( self ) -> I64`

Allocate one object slot. Returns slot index on success, -1 if full. 

#### `function deallocate( self, I64 slot ) -> Void`

Return an object slot to the free list.

**Parameters**:

- `slot` (`I64`)
- `Slot index to free.`

#### `function getObjectSize( self ) -> I64`

Return the fixed object size in bytes. 

#### `function getUsed( self ) -> I64`

Return the number of currently allocated slots. 

#### `function getCapacity( self ) -> I64`

Return total number of available slots. 

#### `function isFull( self ) -> Boolean`

Return whether all slots are allocated. 

#### `function destroy( self ) -> Void`

Free all backing memory.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

