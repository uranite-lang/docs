# uranite.os.arch.x86-64.paging

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
- `uranite.os.arch.x86-64.cpu`
  - `invlpg`
  - `readCr3`
  - `writeCr3`

## const `PAGE_PRESENT`

Present flag (bit 0): the page table entry is valid and the referenced page is in physical memory. 

## const `PAGE_WRITABLE`

Read/Write flag (bit 1): allows write access to the page when set, otherwise read-only. 

## const `PAGE_USER`

User/Supervisor flag (bit 2): allows user-mode (ring 3) access when set, otherwise kernel-only. 

## const `PAGE_WRITE_THROUGH`

Write-Through flag (bit 3): enables write-through caching, writes go directly to memory. 

## const `PAGE_CACHE_DISABLE`

Cache Disable flag (bit 4): disables caching entirely, used for memory-mapped I/O regions. 

## const `PAGE_ACCESSED`

Accessed flag (bit 5): set automatically by processor on read or write, used for page replacement. 

## const `PAGE_DIRTY`

Dirty flag (bit 6): set automatically on write, indicates page must be flushed before eviction. 

## const `PAGE_HUGE`

Page Size flag (bit 7): creates a huge page (2MB at PD level, 1GB at PDPT level). 

## const `PAGE_GLOBAL`

Global flag (bit 8): prevents TLB flush on CR3 reload, used for kernel pages. 

## function `pageNoExecute`

Return the No-Execute flag (bit 63) that prevents instruction fetches from this page, providing hardware-enforced W^X protection. Requires the NX bit to be enabled in the EFER MSR.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function pageNoExecute(  ) -> I64`

Return the No-Execute flag (bit 63) that prevents instruction fetches from this page, providing hardware-enforced W^X protection. Requires the NX bit to be enabled in the EFER MSR.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `PageTableEntry`

Single 8-byte page table entry that combines a physical page frame address with permission and status flags. Physical addresses are 4KB-aligned, so the low 12 bits of each entry are available for flags (present, writable, user, accessed, dirty, etc.) while bits 12-51 hold the physical frame number. This representation is used at all four levels of the x86_64 page table hierarchy (PML4, PDPT, PD, PT).

### Fields

| Name | Type | Access |
|------|------|--------|
| `raw` | `I64` | public |

### Methods

#### `function PageTableEntry( self, I64 raw ) -> Void`

Construct a page table entry from a raw 8-byte value containing both the physical address and flag bits combined into a single integer.

**Parameters**:

- `raw` (`I64`)
- `The raw entry value with physical address in bits 12-51 and`
- `flags in bits 0-11 and bit 63.`

#### `function isPresent( self ) -> Boolean`

Check if the present flag (bit 0) is set, indicating this entry is valid and the referenced page or table exists in physical memory. 

#### `function isWritable( self ) -> Boolean`

Check if the read/write flag (bit 1) is set, indicating write access is allowed to the referenced page. 

#### `function isUser( self ) -> Boolean`

Check if the user/supervisor flag (bit 2) is set, indicating user-mode (ring 3) code can access the referenced page. 

#### `function isHuge( self ) -> Boolean`

Check if the page size flag (bit 7) is set, indicating this entry maps a huge page (2MB or 1GB) instead of pointing to a lower-level page table. 

#### `function address( self ) -> I64`

Extract the physical page frame address from this entry by masking out the flag bits (0-11) and reserved bits (52-62), returning only bits 12-51 which contain the 4KB-aligned physical address.

**Returns**: `I64` — The physical address of the referenced page frame or next-level page table.

#### `function set( self, I64 physAddr, I64 flags ) -> Void`

Set this entry to map to the given physical address with the specified flags. The physical address is masked to bits 12-51 and OR'd with the flag bits to produce the raw entry value.

**Parameters**:

- `physAddr` (`I64`)
- `The 4KB-aligned physical address of the target page frame.`
- `flags` (`I64`)
- `The page table entry flags` (`present, writable, user, etc.`)

#### `function clear( self ) -> Void`

Clear this entry by setting the raw value to zero, marking it as not-present and removing any physical address mapping. 

#### `function flags( self ) -> I64`

Extract and return only the flag bits (bits 0-11) from this entry, masking out the physical address portion.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `PageTable`

Array of 512 page table entries, occupying exactly one 4KB physical page (512 entries x 8 bytes = 4096 bytes). This structure is used at all four levels of the x86_64 page table hierarchy: PML4 (Page Map Level 4), PDPT (Page Directory Pointer Table), PD (Page Directory), and PT (Page Table). Each entry either points to the next-level table or, at the final level, maps directly to a physical page frame.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<I64>` | protect |

### Methods

#### `function PageTable( self ) -> Void`

Construct a page table with all 512 entries initialized to zero, marking every entry as not-present. Entries must be explicitly configured via setEntry before they can be used for address translation.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function getEntry( self, I64 index ) -> PageTableEntry`

Return the page table entry at the given index as a PageTableEntry object that provides accessor methods for the address and flag fields.

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)

**Returns**: — PageTableEntry:
A wrapper around the raw 8-byte entry value at that index.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setEntry( self, I64 index, I64 value ) -> Void`

Write a raw 8-byte value into the page table entry at the given index. The value should be a physical address OR'd with the desired flags.

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)
- `value` (`I64`)
- `The raw entry value containing physical address and flags.`

#### `function clearEntry( self, I64 index ) -> Void`

Clear the page table entry at the given index by setting it to zero, which marks the entry as not-present and removes any address mapping.

**Parameters**:

- `index` (`I64`)
- `The entry index` (`0-511`)

#### `function destroy( self ) -> Void`

Free the underlying memory allocation used to store the 512 page table entries. This page table must not be referenced by any higher-level table entry or active in the CR3 register when destroyed.

## function `pml4Index`

Extract the PML4 table index (bits 39-47) from a 64-bit virtual address, yielding a value 0-511 that selects the top-level page table entry. 

### Methods

#### `function pml4Index( I64 virtualAddr ) -> I64`

Extract the PML4 table index (bits 39-47) from a 64-bit virtual address, yielding a value 0-511 that selects the top-level page table entry. 

## function `pdptIndex`

Extract the Page Directory Pointer Table index (bits 30-38) from a 64-bit virtual address, yielding a value 0-511 that selects the second-level page table entry. 

### Methods

#### `function pdptIndex( I64 virtualAddr ) -> I64`

Extract the Page Directory Pointer Table index (bits 30-38) from a 64-bit virtual address, yielding a value 0-511 that selects the second-level page table entry. 

## function `pdIndex`

Extract the Page Directory index (bits 21-29) from a 64-bit virtual address, yielding a value 0-511 that selects the third-level page table entry. 

### Methods

#### `function pdIndex( I64 virtualAddr ) -> I64`

Extract the Page Directory index (bits 21-29) from a 64-bit virtual address, yielding a value 0-511 that selects the third-level page table entry. 

## function `ptIndex`

Extract the Page Table index (bits 12-20) from a 64-bit virtual address, yielding a value 0-511 that selects the final-level page table entry pointing to the physical frame.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function ptIndex( I64 virtualAddr ) -> I64`

Extract the Page Table index (bits 12-20) from a 64-bit virtual address, yielding a value 0-511 that selects the final-level page table entry pointing to the physical frame.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## const `PAGE_SIZE`

Standard 4KB page size, the smallest granularity supported by x86_64. 

## const `HUGE_PAGE_2MB`

2MB huge page size, created by setting the page size flag at the Page Directory level. 

## const `HUGE_PAGE_1GB`

1GB huge page size, created by setting the page size flag at the PDPT level. 

## function `flushPage`

Flush a single TLB entry for the specified virtual address by invoking the invlpg instruction. This selectively invalidates only the cached translation for one page without affecting other TLB entries.

**Parameters**:

- `virtualAddr` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

### Methods

#### `function flushPage( I64 virtualAddr ) -> Void`

Flush a single TLB entry for the specified virtual address by invoking the invlpg instruction. This selectively invalidates only the cached translation for one page without affecting other TLB entries.

**Parameters**:

- `virtualAddr` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

## function `flushTlb`

Flush the entire Translation Lookaside Buffer by reading the current CR3 value and writing it back. Reloading CR3 causes the processor to discard all cached address translations, which is an expensive operation but necessary when multiple page table entries have been modified.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function flushTlb(  ) -> Void`

Flush the entire Translation Lookaside Buffer by reading the current CR3 value and writing it back. Reloading CR3 causes the processor to discard all cached address translations, which is an expensive operation but necessary when multiple page table entries have been modified.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

