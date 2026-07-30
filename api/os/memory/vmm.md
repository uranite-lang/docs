# uranite.os.memory.vmm

## Table of Contents

- [Imports](#imports)
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
  - [`unmapPage()`](#unmapPage)
  - [`mapRange()`](#mapRange)
  - [`unmapRange()`](#unmapRange)
  - [`translate()`](#translate)
  - [`isMapped()`](#isMapped)
  - [`activate()`](#activate)
  - [`getPml4PhysAddr()`](#getPml4PhysAddr)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.arch.x86-64.cpu`
  - `invlpg`
  - `writeCr3`
- `uranite.os.arch.x86-64.paging`
  - `PAGE_PRESENT`
  - `PAGE_USER`
  - `PAGE_WRITABLE`
  - `PageTable`
  - `pdIndex`
  - `pdptIndex`
  - `pml4Index`
  - `ptIndex`
- `uranite.os.memory.pmm`
  - `PhysicalMemoryManager`

## function `protReadOnly`

Return page protection flags for read-only kernel access (Present bit only). 

### Methods

#### `function protReadOnly(  ) -> I64`

Return page protection flags for read-only kernel access (Present bit only). 

## function `protReadWrite`

Return page protection flags for read-write kernel access (Present and Writable bits). 

### Methods

#### `function protReadWrite(  ) -> I64`

Return page protection flags for read-write kernel access (Present and Writable bits). 

## function `protUser`

Return page protection flags for read-write user-space access (Present, Writable, and User bits). 

### Methods

#### `function protUser(  ) -> I64`

Return page protection flags for read-write user-space access (Present, Writable, and User bits). 

## function `protUserReadOnly`

Return page protection flags for read-only user-space access (Present and User bits).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function protUserReadOnly(  ) -> I64`

Return page protection flags for read-only user-space access (Present and User bits).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `AddressSpace`

Per-process virtual address space that wraps the x86_64 4-level page table hierarchy (PML4 -> PDPT -> PD -> PT). All intermediate page tables are allocated on demand from the Physical Memory Manager. Provides methods to map, unmap, and translate virtual addresses to physical addresses.

### Fields

| Name | Type | Access |
|------|------|--------|
| `pml4` | `PageTable` | protect |
| `pml4PhysAddr` | `I64` | protect |
| `pmm` | `PhysicalMemoryManager` | protect |

### Methods

#### `function AddressSpace( self, PhysicalMemoryManager pmm ) -> Void`

Construct a new address space backed by the given Physical Memory Manager. Allocates a PML4 page table and obtains a physical frame for CR3 loading.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function mapPage( self, I64 virtAddr, I64 physAddr, I64 flags ) -> Boolean`

Map a single 4KB virtual page to a physical address with the given protection flags. Walks the 4-level page table hierarchy, allocating intermediate page tables from the PMM as needed. Returns True on success, or False if the PMM has run out of free frames and cannot allocate a required intermediate page table.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function unmapPage( self, I64 virtAddr ) -> Void`

Unmap a single 4KB virtual page by clearing its page table entry and flushing the TLB entry for that address using the invlpg instruction.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function mapRange( self, I64 virtAddr, I64 physAddr, I64 pageCount, I64 flags ) -> Boolean`

Map a contiguous range of 4KB pages from virtual to physical addresses with the given protection flags. Returns True if all pages were mapped successfully, or False if any single page mapping fails due to PMM frame exhaustion.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unmapRange( self, I64 virtAddr, I64 pageCount ) -> Void`

Unmap a contiguous range of 4KB virtual pages, clearing each page table entry and flushing the corresponding TLB entries.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function translate( self, I64 virtAddr ) -> I64`

Translate a virtual address to its corresponding physical address by performing a software page table walk through all four levels of the x86_64 page hierarchy (PML4 -> PDPT -> PD -> PT). Returns the physical address including the page offset, or 0 if the virtual address is not mapped.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isMapped( self, I64 virtAddr ) -> Boolean`

Check if the given virtual address has a valid page table mapping. 

#### `function activate( self ) -> Void`

Activate this address space by loading its PML4 physical address into the CR3 register. This flushes the entire TLB, which is an expensive operation and should only be done during address space switches between different processes.

#### `function getPml4PhysAddr( self ) -> I64`

Return the physical address of this address space's PML4 page table for CR3 loading.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

