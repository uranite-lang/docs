# uranite.os.memory.address-space

## Table of Contents

- [Imports](#imports)
- [const `PAGE_PRESENT`](#const-page-present)
- [const `PAGE_WRITABLE`](#const-page-writable)
- [const `PAGE_USER`](#const-page-user)
- [function `protReadOnly`](#function-protreadonly)
  - [`protReadOnly()`](#protReadOnly)
- [function `protReadWrite`](#function-protreadwrite)
  - [`protReadWrite()`](#protReadWrite)
- [function `protUser`](#function-protuser)
  - [`protUser()`](#protUser)
- [function `protUserReadOnly`](#function-protuserreadonly)
  - [`protUserReadOnly()`](#protUserReadOnly)
- [class `AddressSpace`](#class-addressspace)
  - [`AddressSpace()`](#AddressSpace)
  - [`mapPage()`](#mapPage)
  - [`mapRange()`](#mapRange)
  - [`unmapPage()`](#unmapPage)
  - [`unmapRange()`](#unmapRange)
  - [`translate()`](#translate)
  - [`isMapped()`](#isMapped)
  - [`activate()`](#activate)
  - [`getRootAddress()`](#getRootAddress)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.memory.pmm`
  - `PhysicalMemoryManager`

## const `PAGE_PRESENT`

## const `PAGE_WRITABLE`

## const `PAGE_USER`

## function `protReadOnly`

Return page protection flags for read-only kernel access. 

### Methods

#### `function protReadOnly(  ) -> I64`

Return page protection flags for read-only kernel access. 

## function `protReadWrite`

Return page protection flags for read-write kernel access. 

### Methods

#### `function protReadWrite(  ) -> I64`

Return page protection flags for read-write kernel access. 

## function `protUser`

Return page protection flags for read-write user-space access. 

### Methods

#### `function protUser(  ) -> I64`

Return page protection flags for read-write user-space access. 

## function `protUserReadOnly`

Return page protection flags for read-only user-space access. 

### Methods

#### `function protUserReadOnly(  ) -> I64`

Return page protection flags for read-only user-space access. 

## class `AddressSpace`

Per-process virtual address space backed by a Physical Memory Manager. Provides map, unmap, and translate operations over the underlying page table structure. Architecture-specific VMM modules (x86-64 PML4, aarch64 TTBR) provide hardware-accelerated implementations; this cross-platform version uses software bookkeeping for portability.

### Fields

| Name | Type | Access |
|------|------|--------|
| `pmm` | `PhysicalMemoryManager` | protect |
| `rootAddr` | `I64` | protect |
| `mappingTable` | `Memory<I64>` | protect |
| `mappingCount` | `I64` | protect |
| `mappingCapacity` | `I64` | protect |

### Methods

#### `function AddressSpace( self, PhysicalMemoryManager pmm ) -> Void`

Construct a new address space backed by the given Physical Memory Manager. Allocates a root page table frame and initializes the mapping table.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function mapPage( self, I64 virtAddr, I64 physAddr, I64 flags ) -> Boolean`

Map a single 4KB virtual page to a physical address with the given protection flags. Returns True on success, False if the mapping table is full.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function mapRange( self, I64 virtAddr, I64 physAddr, I64 pageCount, I64 flags ) -> Boolean`

Map a contiguous range of 4KB pages from virtual to physical addresses with the given protection flags.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unmapPage( self, I64 virtAddr ) -> Void`

Unmap a single 4KB virtual page by clearing its mapping entry.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unmapRange( self, I64 virtAddr, I64 pageCount ) -> Void`

Unmap a contiguous range of 4KB virtual pages.

**Complexity**:
- Time: `O(n*m)`
- Space: `O(1)`

#### `function translate( self, I64 virtAddr ) -> I64`

Translate a virtual address to its corresponding physical address. Returns the physical address or 0 if not mapped.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function isMapped( self, I64 virtAddr ) -> Boolean`

Check if the given virtual address has a valid mapping. 

#### `function activate( self ) -> Void`

Activate this address space on the current CPU. On x86-64 this writes CR3; on aarch64 this writes TTBR0_EL1. In software-only mode this is a no-op.

#### `function getRootAddress( self ) -> I64`

Return the physical address of the root page table.

**Returns**: `I64` — The physical address of the root table frame.

#### `function destroy( self ) -> Void`

Release all memory held by this address space.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

