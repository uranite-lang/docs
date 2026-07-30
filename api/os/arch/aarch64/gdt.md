# uranite.os.arch.aarch64.gdt

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

AArch64 equivalent of a segment descriptor. Stores exception level attributes in a layout compatible with the x86-64 GdtEntry API. On AArch64, these fields represent SPSR configuration for EL transitions rather than segment limits.

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

Construct a descriptor entry with the given attributes, splitting fields in the same manner as x86-64 for API compatibility.

**Parameters**:

- `base` (`I64`)
- `Base address field` (`unused on AArch64, stored for compatibility`)
- `limit` (`I64`)
- `Limit field` (`unused on AArch64, stored for compatibility`)
- `access` (`I64`)
- `Access byte representing exception level and permissions.`
- `flags` (`I64`)
- `Flags nibble for granularity and mode bits.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## struct `GdtPointer`

AArch64 equivalent of the GDTR pointer structure. Stores a base address and limit for the exception vector table (VBAR_EL1) which serves a similar structural role to the x86 GDT pointer.

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

AArch64 exception level descriptor table manager. Provides the same API as the x86-64 GDT class, storing entries that represent exception level configurations instead of segment descriptors.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<GdtEntry>` | protect |
| `count` | `I64` | protect |

### Methods

#### `function Gdt( self ) -> Void`

Construct an empty descriptor table with pre-allocated storage for up to 6 entries.

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

Initialize a standard five-entry table mapping to AArch64 exception levels: null entry, EL1 code (kernel), EL1 data (kernel), EL0 code (user), EL0 data (user). Uses the same access byte values as x86-64 for API compatibility.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Return the number of entries currently stored. 

#### `function destroy( self ) -> Void`

Free the underlying memory allocation for the entry table. 

## function `loadGdt`

On AArch64, writes the base address to VBAR_EL1 (Vector Base Address Register) which is the closest equivalent to loading a GDT — it tells the CPU where to find the exception/interrupt handler table.

**Parameters**:

- `ptr` (`GdtPointer`)
- `The table pointer containing the base address.`

### Methods

#### `function loadGdt( GdtPointer ptr ) -> Void`

On AArch64, writes the base address to VBAR_EL1 (Vector Base Address Register) which is the closest equivalent to loading a GDT — it tells the CPU where to find the exception/interrupt handler table.

**Parameters**:

- `ptr` (`GdtPointer`)
- `The table pointer containing the base address.`

## function `reloadSegments`

No-op on AArch64. There are no segment registers to reload. Exception level transitions are handled by ERET/SVC instructions, not segment register loads.

### Methods

#### `function reloadSegments( I64 codeSelector, I64 dataSelector ) -> Void`

No-op on AArch64. There are no segment registers to reload. Exception level transitions are handled by ERET/SVC instructions, not segment register loads.

## const `KERNEL_CODE_SELECTOR`

## const `KERNEL_DATA_SELECTOR`

## function `userCodeSelector`

Return the user-space code selector value (27) for API compatibility with x86-64. On AArch64 this corresponds to EL0 execution privilege.

**Returns**: `I64` — The user code selector value (27).

### Methods

#### `function userCodeSelector(  ) -> I64`

Return the user-space code selector value (27) for API compatibility with x86-64. On AArch64 this corresponds to EL0 execution privilege.

**Returns**: `I64` — The user code selector value (27).

## function `userDataSelector`

Return the user-space data selector value (35) for API compatibility with x86-64. On AArch64 this corresponds to EL0 data access privilege.

**Returns**: `I64` — The user data selector value (35).

### Methods

#### `function userDataSelector(  ) -> I64`

Return the user-space data selector value (35) for API compatibility with x86-64. On AArch64 this corresponds to EL0 data access privilege.

**Returns**: `I64` — The user data selector value (35).

