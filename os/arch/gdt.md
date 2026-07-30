# uranite.os.arch.gdt

## Table of Contents

- [Imports](#imports)
- [struct `GdtEntry`](#struct-gdtentry)
  - [`GdtEntry()`](#GdtEntry)
- [struct `GdtPointer`](#struct-gdtpointer)
  - [`GdtPointer()`](#GdtPointer)
- [class `Gdt`](#class-gdt)
  - [`Gdt()`](#Gdt)
  - [`addEntry()`](#addEntry)
  - [`setupFlat()`](#setupFlat)
  - [`getCount()`](#getCount)
  - [`getPointer()`](#getPointer)
  - [`destroy()`](#destroy)
- [function `loadGdt`](#function-loadgdt)
  - [`loadGdt()`](#loadGdt)
- [function `reloadSegments`](#function-reloadsegments)
  - [`reloadSegments()`](#reloadSegments)
- [const `KERNEL_CODE_SELECTOR`](#const-kernel-code-selector)
- [const `KERNEL_DATA_SELECTOR`](#const-kernel-data-selector)
- [function `userCodeSelector`](#function-usercodeselector)
  - [`userCodeSelector()`](#userCodeSelector)
- [function `userDataSelector`](#function-userdataselector)
  - [`userDataSelector()`](#userDataSelector)

## Imports

- `uranite.memory.memory`
  - `Memory`

## struct `GdtEntry`

Segment descriptor entry. On x86-64, encodes base/limit/access/flags for hardware segmentation. On AArch64, stores exception level attributes in the same field layout for API compatibility.

### Fields

| Name | Type | Access |
|------|------|--------|
| `limitLow` | `I64` | public |
| `baseLow` | `I64` | public |
| `baseMiddle` | `I64` | public |
| `access` | `I64` | public |
| `granularity` | `I64` | public |
| `baseHigh` | `I64` | public |

### Methods

#### `function GdtEntry( self, I64 base, I64 limit, I64 access, I64 flags ) -> Void`

Construct a descriptor entry by splitting base, limit, access, and flags into the fragmented bit layout required by x86 hardware.

**Parameters**:

- `base` (`I64`)
- `The 32-bit segment base address.`
- `limit` (`I64`)
- `The 20-bit segment limit value.`
- `access` (`I64`)
- `The access byte controlling segment type, DPL, and presence.`
- `flags` (`I64`)
- `The 4-bit flags nibble controlling granularity and mode.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## struct `GdtPointer`

Table pointer loaded into the CPU. On x86-64 this is the GDTR structure (limit + base). On AArch64 the base is written to VBAR_EL1.

### Fields

| Name | Type | Access |
|------|------|--------|
| `limit` | `I64` | public |
| `base` | `I64` | public |

### Methods

#### `function GdtPointer( self, I64 limit, I64 base ) -> Void`

Construct a table pointer with the given limit and base address.

**Parameters**:

- `limit` (`I64`)
- `The table size in bytes minus one.`
- `base` (`I64`)
- `The base address of the table.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `Gdt`

Descriptor table manager holding segment/exception-level entries and providing standard flat-mode initialization.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<GdtEntry>` | protect |
| `count` | `I64` | protect |

### Methods

#### `function Gdt( self ) -> Void`

Construct an empty descriptor table with storage for up to 6 entries.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function addEntry( self, GdtEntry entry ) -> I64`

Append a descriptor entry and return its selector value (index * 8).

**Parameters**:

- `entry` (`GdtEntry`)
- `The descriptor entry to append.`

**Returns**: `I64` — The selector value for the newly added entry.

#### `function setupFlat( self ) -> Void`

Initialize a standard five-entry flat table: null descriptor, kernel code, kernel data, user code, user data. On x86-64 these are segment descriptors; on AArch64 they represent EL1/EL0 privilege boundaries.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Return the number of entries currently stored. 

#### `function getPointer( self ) -> GdtPointer`

Construct a table pointer for loading into the CPU.

**Returns**: `GdtPointer` — A pointer with limit = (count * 8) - 1 and base = entry storage address.

#### `function destroy( self ) -> Void`

Free the underlying memory allocation for the entry table. 

## function `loadGdt`

Load the descriptor table into the CPU. On x86-64 this executes lgdt. On AArch64 this writes VBAR_EL1.

**Parameters**:

- `ptr` (`GdtPointer`)
- `The table pointer containing the base address and size limit.`

### Methods

#### `function loadGdt( GdtPointer ptr ) -> Void`

Load the descriptor table into the CPU. On x86-64 this executes lgdt. On AArch64 this writes VBAR_EL1.

**Parameters**:

- `ptr` (`GdtPointer`)
- `The table pointer containing the base address and size limit.`

## function `reloadSegments`

Reload segment registers after a table change. On x86-64 this loads DS/ES/SS. On AArch64 this is a no-op (no segment registers exist).

**Parameters**:

- `codeSelector` (`I64`)
- `The code segment selector.`
- `dataSelector` (`I64`)
- `The data segment selector to load into DS, ES, and SS.`

### Methods

#### `function reloadSegments( I64 codeSelector, I64 dataSelector ) -> Void`

Reload segment registers after a table change. On x86-64 this loads DS/ES/SS. On AArch64 this is a no-op (no segment registers exist).

**Parameters**:

- `codeSelector` (`I64`)
- `The code segment selector.`
- `dataSelector` (`I64`)
- `The data segment selector to load into DS, ES, and SS.`

## const `KERNEL_CODE_SELECTOR`

## const `KERNEL_DATA_SELECTOR`

## function `userCodeSelector`

Return the user-space code selector value (24 | 3 = 27).

**Returns**: `I64` — The user code segment selector value.

### Methods

#### `function userCodeSelector(  ) -> I64`

Return the user-space code selector value (24 | 3 = 27).

**Returns**: `I64` — The user code segment selector value.

## function `userDataSelector`

Return the user-space data selector value (32 | 3 = 35).

**Returns**: `I64` — The user data segment selector value.

### Methods

#### `function userDataSelector(  ) -> I64`

Return the user-space data selector value (32 | 3 = 35).

**Returns**: `I64` — The user data segment selector value.

