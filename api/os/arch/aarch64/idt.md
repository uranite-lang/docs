# uranite.os.arch.aarch64.idt

## Table of Contents

- [Imports](#imports)
- [class `IdtEntry`](#class-idtentry)
  - [`IdtEntry()`](#IdtEntry)
  - [`setHandler()`](#setHandler)
  - [`getHandler()`](#getHandler)
- [struct `IdtPointer`](#struct-idtpointer)
  - [`IdtPointer()`](#IdtPointer)
- [enum `GateType`](#enum-gatetype)
- [enum `InterruptVector`](#enum-interruptvector)
- [const `IRQ_BASE`](#const-irq-base)
- [class `Idt`](#class-idt)
  - [`Idt()`](#Idt)
  - [`setGate()`](#setGate)
  - [`setGateWithIst()`](#setGateWithIst)
  - [`getEntry()`](#getEntry)
  - [`destroy()`](#destroy)
- [function `loadIdt`](#function-loadidt)
  - [`loadIdt()`](#loadIdt)

## Imports

- `uranite.memory.memory`
  - `Memory`

## class `IdtEntry`

AArch64 exception vector entry. Stores the handler address and attributes for a single exception vector slot. On ARM, each vector slot is 128 bytes (32 instructions) of inline code rather than a pointer, but this abstraction stores handler addresses for compatibility.

### Fields

| Name | Type | Access |
|------|------|--------|
| `offsetLow` | `I64` | public |
| `selector` | `I64` | public |
| `ist` | `I64` | public |
| `typeAttr` | `I64` | public |
| `offsetMiddle` | `I64` | public |
| `offsetHigh` | `I64` | public |
| `reserved` | `I64` | public |

### Methods

#### `function IdtEntry( self ) -> Void`

Construct a zeroed vector entry with no handler installed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setHandler( self, I64 handler, I64 selector, I64 typeAttr, I64 ist ) -> Void`

Configure this entry with an exception handler address and attributes.

**Parameters**:

- `handler` (`I64`)
- `The virtual address of the exception handler function.`
- `selector` (`I64`)
- `Selector value` (`unused on AArch64, stored for compatibility`)
- `typeAttr` (`I64`)
- `Gate type attributes` (`mapped to exception routing config`)
- `ist` (`I64`)
- `Stack index` (`mapped to SP_ELx selection on AArch64`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getHandler( self ) -> I64`

Reconstruct and return the full 64-bit handler address.

**Returns**: — I64:
The handler function address.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## struct `IdtPointer`

AArch64 exception vector table pointer, corresponding to the VBAR_EL1 register contents. The base field holds the 2KB-aligned vector table address.

### Fields

| Name | Type | Access |
|------|------|--------|
| `limit` | `I64` | public |
| `base` | `I64` | public |

### Methods

#### `function IdtPointer( self, I64 limit, I64 base ) -> Void`

Construct a vector table pointer.

**Parameters**:

- `limit` (`I64`)
- `Table size in bytes minus one.`
- `base` (`I64`)
- `Base address of the exception vector table` (`must be 2KB-aligned`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## enum `GateType`

Exception routing attributes mapped from x86-64 gate types to AArch64 exception level routing. The numeric values match x86-64 for test compatibility.

## enum `InterruptVector`

AArch64 exception syndrome values mapped to x86-64-compatible vector numbers for cross-platform kernel code. ARM exceptions are classified by ESR_EL1 syndrome codes rather than fixed vector numbers, but this enum provides the same constants used by x86-64 code.

## const `IRQ_BASE`

## class `Idt`

AArch64 exception vector table manager. Provides the same API as the x86-64 IDT class, storing 256 vector entries for compatibility. On actual hardware, the ARM vector table has a fixed 16-entry layout (4 exception types x 4 source contexts), but this abstraction allows the same test patterns.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<IdtEntry>` | protect |
| `count` | `I64` | protect |

### Methods

#### `function Idt( self ) -> Void`

Construct a vector table with 256 zeroed entries.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function setGate( self, I64 vector, I64 handler, I64 selector, I64 typeAttr ) -> Void`

Install an exception handler for the specified vector number.

**Parameters**:

- `vector` (`I64`)
- `The vector number` (`0-255`)
- `handler` (`I64`)
- `The handler function address.`
- `selector` (`I64`)
- `Code selector` (`unused on AArch64`)
- `typeAttr` (`I64`)
- `Gate type attributes.`

#### `function setGateWithIst( self, I64 vector, I64 handler, I64 selector, I64 typeAttr, I64 ist ) -> Void`

Install an exception handler with dedicated stack selection.

**Parameters**:

- `vector` (`I64`)
- `The vector number` (`0-255`)
- `handler` (`I64`)
- `The handler function address.`
- `selector` (`I64`)
- `Code selector` (`unused on AArch64`)
- `typeAttr` (`I64`)
- `Gate type attributes.`
- `ist` (`I64`)
- `Stack index for SP_ELx selection.`

#### `function getEntry( self, I64 vector ) -> IdtEntry`

Return the vector entry for the specified index.

**Parameters**:

- `vector` (`I64`)
- `The vector number` (`0-255`)

**Returns**: `IdtEntry` — The entry for the given vector.

#### `function destroy( self ) -> Void`

Free the underlying memory allocation for the vector table. 

## function `loadIdt`

Load the exception vector table base address into VBAR_EL1. This is the AArch64 equivalent of the x86-64 lidt instruction. The table must be 2KB-aligned.

**Parameters**:

- `ptr` (`IdtPointer`)
- `The vector table pointer containing the base address.`

### Methods

#### `function loadIdt( IdtPointer ptr ) -> Void`

Load the exception vector table base address into VBAR_EL1. This is the AArch64 equivalent of the x86-64 lidt instruction. The table must be 2KB-aligned.

**Parameters**:

- `ptr` (`IdtPointer`)
- `The vector table pointer containing the base address.`

