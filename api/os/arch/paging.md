# uranite.os.arch.paging

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
- `uranite.os.arch.cpu`
  - `disableInterrupts`
  - `enableInterrupts`
  - `halt`

## const `PAGE_PRESENT`

Valid/present flag (bit 0): the entry references a valid page or table. 

## const `PAGE_WRITABLE`

Read/write flag (bit 1): allows write access to the page. 

## const `PAGE_USER`

User flag (bit 2): allows user-mode access to the page. 

## const `PAGE_WRITE_THROUGH`

Write-through caching attribute (bit 3). 

## const `PAGE_CACHE_DISABLE`

Cache disable flag (bit 4): disables caching for MMIO regions. 

## const `PAGE_ACCESSED`

Accessed flag (bit 5): set by hardware on first access. 

## const `PAGE_DIRTY`

Dirty flag (bit 6): set by hardware on write. 

## const `PAGE_HUGE`

Page size / block descriptor flag (bit 7): creates a 2MB or 1GB mapping. 

## const `PAGE_GLOBAL`

Global flag (bit 8): prevents TLB flush on address space switch. 

## function `pageNoExecute`

Return the no-execute flag. On x86-64 this is bit 63 (NX). On AArch64 this is bit 54 (UXN).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function pageNoExecute(  ) -> I64`

Return the no-execute flag. On x86-64 this is bit 63 (NX). On AArch64 this is bit 54 (UXN).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `PageTableEntry`

Single 8-byte page table entry combining a physical page frame address with permission and status flags. Bits 12-51 hold the physical frame number; bits 0-11 hold flags.

### Fields

| Name | Type | Access |
|------|------|--------|
| `raw` | `I64` | public |

### Methods

#### `function PageTableEntry( self, I64 raw ) -> Void`

Construct a page table entry from a raw 8-byte value.

**Parameters**:

- `raw` (`I64`)
- `The raw entry value with address and flag bits.`

#### `function isPresent( self ) -> Boolean`

Check if the valid/present flag (bit 0) is set. 

#### `function isWritable( self ) -> Boolean`

Check if the writable flag (bit 1) is set. 

#### `function isUser( self ) -> Boolean`

Check if the user-accessible flag (bit 2) is set. 

#### `function isHuge( self ) -> Boolean`

Check if the page size / block descriptor flag (bit 7) is set. 

#### `function address( self ) -> I64`

Extract the physical page frame address (bits 12-51).

**Returns**: `I64` — The 4KB-aligned physical address.

#### `function set( self, I64 physAddr, I64 flags ) -> Void`

Set this entry to map to the given physical address with flags.

**Parameters**:

- `physAddr` (`I64`)
- `The 4KB-aligned physical address.`
- `flags` (`I64`)
- `The page table entry flags.`

#### `function clear( self ) -> Void`

Clear this entry (mark as not-present). 

#### `function flags( self ) -> I64`

Extract only the flag bits (bits 0-11).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `PageTable`

Array of 512 page table entries (one 4KB page). Used at all four levels of the translation hierarchy.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<I64>` | protect |

### Methods

#### `function PageTable( self ) -> Void`

Construct a page table with all 512 entries zeroed (not-present).

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function getEntry( self, I64 index ) -> PageTableEntry`

Return the entry at the given index as a PageTableEntry object.

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)

**Returns**: `PageTableEntry` — A wrapper around the raw entry value.

#### `function setEntry( self, I64 index, I64 value ) -> Void`

Write a raw value into the entry at the given index.

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)
- `value` (`I64`)
- `The raw entry value.`

#### `function clearEntry( self, I64 index ) -> Void`

Clear the entry at the given index.

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)

#### `function destroy( self ) -> Void`

Free the underlying memory allocation. 

## function `pml4Index`

Extract the L0/PML4 index (bits 39-47) from a virtual address. 

### Methods

#### `function pml4Index( I64 virtualAddr ) -> I64`

Extract the L0/PML4 index (bits 39-47) from a virtual address. 

## function `pdptIndex`

Extract the L1/PDPT index (bits 30-38) from a virtual address. 

### Methods

#### `function pdptIndex( I64 virtualAddr ) -> I64`

Extract the L1/PDPT index (bits 30-38) from a virtual address. 

## function `pdIndex`

Extract the L2/PD index (bits 21-29) from a virtual address. 

### Methods

#### `function pdIndex( I64 virtualAddr ) -> I64`

Extract the L2/PD index (bits 21-29) from a virtual address. 

## function `ptIndex`

Extract the L3/PT index (bits 12-20) from a virtual address.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function ptIndex( I64 virtualAddr ) -> I64`

Extract the L3/PT index (bits 12-20) from a virtual address.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## const `PAGE_SIZE`

Standard 4KB page size. 

## const `HUGE_PAGE_2MB`

2MB huge/block page size (L2 block descriptor). 

## const `HUGE_PAGE_1GB`

1GB huge/block page size (L1 block descriptor). 

## function `flushPage`

Invalidate a single TLB entry for the specified virtual address.

**Parameters**:

- `virtualAddr` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

### Methods

#### `function flushPage( I64 virtualAddr ) -> Void`

Invalidate a single TLB entry for the specified virtual address.

**Parameters**:

- `virtualAddr` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

## function `flushTlb`

Flush the entire TLB. On x86-64 this reloads CR3. On AArch64 this executes TLBI VMALLE1IS.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function flushTlb(  ) -> Void`

Flush the entire TLB. On x86-64 this reloads CR3. On AArch64 this executes TLBI VMALLE1IS.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

