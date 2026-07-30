# uranite.os.arch.aarch64.paging

## Table of Contents

- [Imports](#imports)
- [const `PAGE_PRESENT`](#const-page-present)
- [const `PAGE_WRITABLE`](#const-page-writable)
- [const `PAGE_USER`](#const-page-user)
- [const `PAGE_WRITE_THROUGH`](#const-page-write-through)
- [const `PAGE_CACHE_DISABLE`](#const-page-cache-disable)
- [const `PAGE_ACCESSED`](#const-page-accessed)
- [const `PAGE_DIRTY`](#const-page-dirty)
- [const `PAGE_HUGE`](#const-page-huge)
- [const `PAGE_GLOBAL`](#const-page-global)
- [function `pageNoExecute`](#function-pagenoexecute)
  - [`pageNoExecute()`](#pageNoExecute)
- [class `PageTableEntry`](#class-pagetableentry)
  - [`PageTableEntry()`](#PageTableEntry)
  - [`isPresent()`](#isPresent)
  - [`isWritable()`](#isWritable)
  - [`isUser()`](#isUser)
  - [`isHuge()`](#isHuge)
  - [`address()`](#address)
  - [`set()`](#set)
  - [`clear()`](#clear)
  - [`flags()`](#flags)
- [class `PageTable`](#class-pagetable)
  - [`PageTable()`](#PageTable)
  - [`getEntry()`](#getEntry)
  - [`setEntry()`](#setEntry)
  - [`clearEntry()`](#clearEntry)
  - [`destroy()`](#destroy)
- [function `pml4Index`](#function-pml4index)
  - [`pml4Index()`](#pml4Index)
- [function `pdptIndex`](#function-pdptindex)
  - [`pdptIndex()`](#pdptIndex)
- [function `pdIndex`](#function-pdindex)
  - [`pdIndex()`](#pdIndex)
- [function `ptIndex`](#function-ptindex)
  - [`ptIndex()`](#ptIndex)
- [const `PAGE_SIZE`](#const-page-size)
- [const `HUGE_PAGE_2MB`](#const-huge-page-2mb)
- [const `HUGE_PAGE_1GB`](#const-huge-page-1gb)
- [function `flushPage`](#function-flushpage)
  - [`flushPage()`](#flushPage)
- [function `flushTlb`](#function-flushtlb)
  - [`flushTlb()`](#flushTlb)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.arch.aarch64.cpu`
  - `invlpg`
  - `readTtbr0`
  - `writeTtbr0`

## const `PAGE_PRESENT`

Valid bit (bit 0): the translation table entry is valid. 

## const `PAGE_WRITABLE`

Read/Write flag (bit 1): maps to AP[2]=0 (read-write). On AArch64, AP bits in the descriptor control access permissions. This constant provides x86-compatible flag semantics.

## const `PAGE_USER`

User flag (bit 2): maps to AP[1]=1 (EL0 accessible). 

## const `PAGE_WRITE_THROUGH`

Write-through caching attribute (maps to MAIR index selection). 

## const `PAGE_CACHE_DISABLE`

Cache disable attribute (maps to device-nGnRnE MAIR index). 

## const `PAGE_ACCESSED`

Access flag (bit 10 in ARM descriptor): set on first access. 

## const `PAGE_DIRTY`

Dirty bit (DBM): set automatically on write if hardware dirty management enabled. 

## const `PAGE_HUGE`

Block descriptor flag: creates a 2MB block at L2 or 1GB block at L1. 

## const `PAGE_GLOBAL`

Non-Global bit inverted: nG=0 means global (not flushed on ASID switch). 

## function `pageNoExecute`

Return the execute-never flag (UXN/PXN). On AArch64 this is bit 54 (UXN) which prevents user-space execution from this page.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function pageNoExecute(  ) -> I64`

Return the execute-never flag (UXN/PXN). On AArch64 this is bit 54 (UXN) which prevents user-space execution from this page.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `PageTableEntry`

Single 8-byte translation table descriptor. On AArch64 with 4KB granule, bits 47:12 hold the output address (physical page frame), and bits 11:0 plus upper bits hold attributes (valid, table/block, AP, SH, AF, etc.). This class uses the same flag layout as x86-64 for API compatibility.

### Fields

| Name | Type | Access |
|------|------|--------|
| `raw` | `I64` | public |

### Methods

#### `function PageTableEntry( self, I64 raw ) -> Void`

Construct a page table entry from a raw descriptor value.

**Parameters**:

- `raw` (`I64`)
- `The raw descriptor value with output address and attribute bits.`

#### `function isPresent( self ) -> Boolean`

Check if the valid bit (bit 0) is set, indicating this descriptor is active. 

#### `function isWritable( self ) -> Boolean`

Check if the writable flag (bit 1) is set. 

#### `function isUser( self ) -> Boolean`

Check if the user-accessible flag (bit 2) is set. 

#### `function isHuge( self ) -> Boolean`

Check if this is a block descriptor (huge page). 

#### `function address( self ) -> I64`

Extract the output address (physical page frame) from this descriptor. Masks bits 47:12, yielding the 4KB-aligned physical address.

**Returns**: `I64` — The physical address of the referenced page frame or next-level table.

#### `function set( self, I64 physAddr, I64 flags ) -> Void`

Set this descriptor to map to the given physical address with flags.

**Parameters**:

- `physAddr` (`I64`)
- `The 4KB-aligned physical address.`
- `flags` (`I64`)
- `The descriptor attribute flags.`

#### `function clear( self ) -> Void`

Clear this descriptor, marking it as invalid. 

#### `function flags( self ) -> I64`

Extract the attribute flag bits (bits 0-11) from this descriptor.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `PageTable`

Array of 512 translation table descriptors occupying one 4KB page. Used at all four levels of AArch64 4KB-granule translation: L0 (equivalent to PML4), L1 (PDPT), L2 (PD), and L3 (PT).

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<I64>` | protect |

### Methods

#### `function PageTable( self ) -> Void`

Construct a translation table with all 512 entries zeroed (invalid).

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function getEntry( self, I64 index ) -> PageTableEntry`

Return the descriptor at the given index as a PageTableEntry object.

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)

**Returns**: `PageTableEntry` — A wrapper around the raw descriptor value.

#### `function setEntry( self, I64 index, I64 value ) -> Void`

Write a raw descriptor value at the given index.

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)
- `value` (`I64`)
- `The raw descriptor value.`

#### `function clearEntry( self, I64 index ) -> Void`

Clear the entry at the given index (mark invalid).

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)

#### `function destroy( self ) -> Void`

Free the underlying memory allocation for this translation table. 

## function `pml4Index`

Extract the L0 table index (bits 39-47) from a virtual address. 

### Methods

#### `function pml4Index( I64 virtualAddr ) -> I64`

Extract the L0 table index (bits 39-47) from a virtual address. 

## function `pdptIndex`

Extract the L1 table index (bits 30-38) from a virtual address. 

### Methods

#### `function pdptIndex( I64 virtualAddr ) -> I64`

Extract the L1 table index (bits 30-38) from a virtual address. 

## function `pdIndex`

Extract the L2 table index (bits 21-29) from a virtual address. 

### Methods

#### `function pdIndex( I64 virtualAddr ) -> I64`

Extract the L2 table index (bits 21-29) from a virtual address. 

## function `ptIndex`

Extract the L3 table index (bits 12-20) from a virtual address.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function ptIndex( I64 virtualAddr ) -> I64`

Extract the L3 table index (bits 12-20) from a virtual address.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## const `PAGE_SIZE`

Standard 4KB page size (smallest AArch64 granule). 

## const `HUGE_PAGE_2MB`

2MB block descriptor size at L2. 

## const `HUGE_PAGE_1GB`

1GB block descriptor size at L1. 

## function `flushPage`

Invalidate a single TLB entry for the specified virtual address via TLBI VAE1IS + DSB + ISB.

**Parameters**:

- `virtualAddr` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

### Methods

#### `function flushPage( I64 virtualAddr ) -> Void`

Invalidate a single TLB entry for the specified virtual address via TLBI VAE1IS + DSB + ISB.

**Parameters**:

- `virtualAddr` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

## function `flushTlb`

Invalidate the entire TLB via TLBI VMALLE1IS + DSB + ISB. This is expensive but necessary when multiple page table entries change.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function flushTlb(  ) -> Void`

Invalidate the entire TLB via TLBI VMALLE1IS + DSB + ISB. This is expensive but necessary when multiple page table entries change.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

