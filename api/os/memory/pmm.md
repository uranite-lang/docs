# uranite.os.memory.pmm

## Table of Contents

- [Imports](#imports)
- [class `PhysicalMemoryManager`](#class-physicalmemorymanager)
  - [`PhysicalMemoryManager()`](#PhysicalMemoryManager)
  - [`allocFrame()`](#allocFrame)
  - [`freeFrame()`](#freeFrame)
  - [`reserveRegion()`](#reserveRegion)
  - [`freeRegion()`](#freeRegion)
  - [`allocContiguous()`](#allocContiguous)
  - [`usedCount()`](#usedCount)
  - [`freeCount()`](#freeCount)
  - [`totalCount()`](#totalCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## class `PhysicalMemoryManager`

Bitmap-based physical frame allocator for managing 4KB physical memory pages. Each bit in the bitmap represents one 4KB frame: a set bit means the frame is in use, and a clear bit means it is free. The allocator is bootstrap-safe, meaning the bitmap region itself must be pre-allocated before initialization. All frames start as used, and the bootloader's memory map should call freeRegion() to mark usable physical ranges as available.

### Fields

| Name | Type | Access |
|------|------|--------|
| `bitmap` | `Memory<I64>` | protect |
| `pool` | `Memory<I64>` | protect |
| `poolBase` | `I64` | protect |
| `totalFrames` | `I64` | protect |
| `usedFrames` | `I64` | protect |
| `bitmapSize` | `I64` | protect |

### Methods

#### `function PhysicalMemoryManager( self, I64 memorySize ) -> Void`

Initialize the physical memory manager with the total amount of physical memory in bytes. Computes the number of 4KB frames and allocates a bitmap to track their usage. A real heap-backed memory pool is allocated so that returned frame addresses are valid dereferenceable pointers. All frames start as free and available.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function allocFrame( self ) -> I64`

Allocate a single 4KB physical frame by scanning the bitmap for the first clear bit. Returns the physical address of the allocated frame, or 0 if no free frames are available.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function freeFrame( self, I64 physAddr ) -> Void`

Free a single 4KB physical frame by clearing its corresponding bit in the bitmap. The physical address is converted to a frame index and the appropriate bit is cleared if it was set.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function reserveRegion( self, I64 physAddr, I64 frameCount ) -> Void`

Reserve a contiguous region of physical frames by marking them as used. Used to protect reserved memory areas such as the kernel image, MMIO regions, and ACPI tables from being allocated by the frame allocator.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function freeRegion( self, I64 physAddr, I64 frameCount ) -> Void`

Mark a contiguous region of physical frames as free and available for allocation. Typically called during boot with ranges obtained from the bootloader's memory map to indicate usable physical memory.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function allocContiguous( self, I64 frameCount ) -> I64`

Allocate a contiguous run of N physical frames by scanning the bitmap for a sequence of clear bits. Returns the base physical address of the allocated region, or 0 if no contiguous run of the requested size is available. The allocated frames are automatically reserved.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function usedCount( self ) -> I64`

Return the number of physical frames currently in use. 

#### `function freeCount( self ) -> I64`

Return the number of physical frames currently available for allocation. 

#### `function totalCount( self ) -> I64`

Return the total number of physical frames managed by this allocator. 

#### `function destroy( self ) -> Void`

Free the bitmap and pool memory used by this allocator.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

