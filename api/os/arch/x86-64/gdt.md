# uranite.os.arch.x86-64.gdt

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

Single 8-byte segment descriptor entry in the Global Descriptor Table. In x86_64 long mode, the base address and segment limit fields are largely ignored because the processor enforces a flat address space with paging. However, the access byte and flags nibble remain significant for defining the privilege level (ring 0 vs ring 3), segment type (code vs data), and execution mode (64-bit long mode vs 32-bit compatibility mode).

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

Construct a GDT entry by splitting the base address, limit, access byte, and flags into the fragmented bit layout required by the x86 hardware. The base is split across baseLow (bits 0-15), baseMiddle (bits 16-23), and baseHigh (bits 24-31). The limit is split across limitLow (bits 0-15) and the lower nibble of the granularity byte (bits 16-19). The flags occupy the upper nibble of the granularity byte.

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
- Time: `O(n)`
- Space: `O(1)`

## struct `GdtPointer`

Descriptor for the GDTR register, loaded by the lgdt instruction. Contains a 2-byte limit field specifying the GDT size minus one in bytes, and an 8-byte linear base address pointing to the start of the GDT in memory. The processor uses this structure to locate and bounds-check all segment descriptor lookups performed during memory access and privilege transitions.

### Fields

| Name | Type | Access |
|------|------|--------|
| `limit` | `I64` | public |
| `base` | `I64` | public |

### Methods

#### `function GdtPointer( self, I64 limit, I64 base ) -> Void`

Construct a GDT pointer with the given size limit and base address. The limit should be set to the total GDT size in bytes minus one, and the base should point to the linear address of the first GDT entry in memory.

**Parameters**:

- `limit` (`I64`)
- `The GDT size in bytes minus one.`
- `base` (`I64`)
- `The linear address of the first GDT entry.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Gdt`

Global Descriptor Table manager that holds an array of segment descriptors and provides methods to populate a standard x86_64 long-mode GDT layout. The access byte flags control segment properties: Present (0x80), DPL ring level (0x00 for kernel, 0x60 for user), Segment type (0x10 for code/data), Executable (0x08), Read/Write (0x02), and Accessed (0x01). The flags nibble controls granularity (0x8 for 4KB pages) and Long mode (0x2 for 64-bit code). Standard layout places null at selector 0x00, kernel code at 0x08, kernel data at 0x10, user code at 0x18, and user data at 0x20.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<GdtEntry>` | protect |
| `count` | `I64` | protect |

### Methods

#### `function Gdt( self ) -> Void`

Construct an empty GDT with pre-allocated storage for up to 6 segment descriptor entries. The entry count starts at zero and is incremented as descriptors are added via addEntry or setupFlat.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function addEntry( self, GdtEntry entry ) -> I64`

Append a segment descriptor entry to the GDT and return its selector value, which is the entry index multiplied by 8 (each descriptor is 8 bytes). The selector value is what gets loaded into segment registers (CS, DS, SS, ES) to reference this particular descriptor.

**Parameters**:

- `entry` (`GdtEntry`)
- `The segment descriptor entry to append.`

**Returns**: `I64` — The segment selector value for the newly added entry.

#### `function setupFlat( self ) -> Void`

Initialize standard x86_64 long-mode GDT with five entries: a mandatory null descriptor at selector 0x00, kernel code segment at 0x08 (present, executable, readable, 64-bit long mode), kernel data segment at 0x10 (present, writable), user code segment at 0x18 (present, DPL 3, executable, readable, 64-bit long mode), and user data segment at 0x20 (present, DPL 3, writable). All segments use a flat address space with full 4GB limit, though in long mode the base and limit are ignored by the processor for code and data segments.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Return the number of segment descriptor entries currently stored in this GDT, which corresponds to the next available slot index.

#### `function destroy( self ) -> Void`

Free the underlying memory allocation used to store the GDT entries. This should only be called when the GDT is no longer loaded into the GDTR, as freeing an active GDT will cause undefined behavior on the next segment register access or privilege transition.

## function `loadGdt`

Load the Global Descriptor Table into the GDTR register using the lgdt instruction. The GdtPointer structure provides the base address and size limit of the GDT to the processor. After loading, segment registers must be reloaded to activate the new segment descriptors.

**Parameters**:

- `ptr` (`GdtPointer`)
- `The GDT pointer containing the base address and size limit.`

### Methods

#### `function loadGdt( GdtPointer ptr ) -> Void`

Load the Global Descriptor Table into the GDTR register using the lgdt instruction. The GdtPointer structure provides the base address and size limit of the GDT to the processor. After loading, segment registers must be reloaded to activate the new segment descriptors.

**Parameters**:

- `ptr` (`GdtPointer`)
- `The GDT pointer containing the base address and size limit.`

## function `reloadSegments`

Reload the data segment registers (DS, ES, SS) with the given data selector after a GDT change. This ensures the processor uses the updated segment descriptors from the newly loaded GDT. The code segment register (CS) is typically reloaded via a far jump or far return, which cannot be expressed as a single inline assembly instruction in all cases.

**Parameters**:

- `codeSelector` (`I64`)
- `The GDT selector for the code segment` (`used for far jump if applicable`)
- `dataSelector` (`I64`)
- `The GDT selector for the data segment to load into DS, ES, and SS.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function reloadSegments( I64 codeSelector, I64 dataSelector ) -> Void`

Reload the data segment registers (DS, ES, SS) with the given data selector after a GDT change. This ensures the processor uses the updated segment descriptors from the newly loaded GDT. The code segment register (CS) is typically reloaded via a far jump or far return, which cannot be expressed as a single inline assembly instruction in all cases.

**Parameters**:

- `codeSelector` (`I64`)
- `The GDT selector for the code segment` (`used for far jump if applicable`)
- `dataSelector` (`I64`)
- `The GDT selector for the data segment to load into DS, ES, and SS.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## const `KERNEL_CODE_SELECTOR`

Segment selector for the kernel code segment at GDT index 1 (offset 0x08). Used in CS when executing kernel-mode code in ring 0.

## const `KERNEL_DATA_SELECTOR`

Segment selector for the kernel data segment at GDT index 2 (offset 0x10). Loaded into DS, ES, and SS when executing in kernel mode at ring 0.

## function `userCodeSelector`

Return the segment selector value for the user code segment, which is at GDT index 3 (offset 0x18) with the low 2 bits set to 3 indicating ring 3 privilege level. This selector is loaded into CS when transitioning to user-mode code via sysret or iretq.

**Returns**: `I64` — The user code segment selector value (27).

### Methods

#### `function userCodeSelector(  ) -> I64`

Return the segment selector value for the user code segment, which is at GDT index 3 (offset 0x18) with the low 2 bits set to 3 indicating ring 3 privilege level. This selector is loaded into CS when transitioning to user-mode code via sysret or iretq.

**Returns**: `I64` — The user code segment selector value (27).

## function `userDataSelector`

Return the segment selector value for the user data segment, which is at GDT index 4 (offset 0x20) with the low 2 bits set to 3 indicating ring 3 privilege level. This selector is loaded into DS, ES, and SS for user-mode processes.

**Returns**: — I64:
The user data segment selector value (35).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function userDataSelector(  ) -> I64`

Return the segment selector value for the user data segment, which is at GDT index 4 (offset 0x20) with the low 2 bits set to 3 indicating ring 3 privilege level. This selector is loaded into DS, ES, and SS for user-mode processes.

**Returns**: — I64:
The user data segment selector value (35).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

