# uranite.os.arch.idt

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
  - [`getPointer()`](#getPointer)
  - [`destroy()`](#destroy)
- [function `loadIdt`](#function-loadidt)
  - [`loadIdt()`](#loadIdt)

## Imports

- `uranite.memory.memory`
  - `Memory`

## class `IdtEntry`

Interrupt/exception gate descriptor entry. Stores the handler address split across three offset fields (x86-64 legacy layout) plus selector, IST index, and gate type attributes.

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

Construct a zeroed entry with no handler installed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setHandler( self, I64 handler, I64 selector, I64 typeAttr, I64 ist ) -> Void`

Configure this entry with a handler address and attributes.

**Parameters**:

- `handler` (`I64`)
- `The 64-bit virtual address of the handler function.`
- `selector` (`I64`)
- `The code segment selector` (`x86-64`)
- `typeAttr` (`I64`)
- `Gate type and attribute byte.`
- `ist` (`I64`)
- `IST index for dedicated stack switching` (`1-7`)

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

Table pointer for the interrupt descriptor table. On x86-64 this is the IDTR structure. On AArch64 the base is written to VBAR_EL1.

### Fields

| Name | Type | Access |
|------|------|--------|
| `limit` | `I64` | public |
| `base` | `I64` | public |

### Methods

#### `function IdtPointer( self, I64 limit, I64 base ) -> Void`

Construct an IDT pointer with the given size limit and base address.

**Parameters**:

- `limit` (`I64`)
- `The table size in bytes minus one.`
- `base` (`I64`)
- `The base address of the first entry.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## enum `GateType`

Gate type attribute values encoding the gate type, DPL, and present bit. On AArch64 these map to exception routing configuration.

## enum `InterruptVector`

CPU exception vector numbers. Vectors 0-31 are processor-reserved exceptions. On AArch64 these map to ESR_EL1 syndrome codes conceptually.

## const `IRQ_BASE`

Base vector number where hardware IRQs start. 

## class `Idt`

Interrupt/exception vector table containing 256 gate descriptor entries. Covers all interrupt vector numbers on x86-64. On AArch64 the fixed 16-entry vector table is abstracted behind this 256-entry interface for compatibility.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<IdtEntry>` | protect |
| `count` | `I64` | protect |

### Methods

#### `function Idt( self ) -> Void`

Construct a full table with 256 zeroed entries.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function setGate( self, I64 vector, I64 handler, I64 selector, I64 typeAttr ) -> Void`

Install a handler for the specified vector number using IST 0.

**Parameters**:

- `vector` (`I64`)
- `The vector number` (`0-255`)
- `handler` (`I64`)
- `The handler function address.`
- `selector` (`I64`)
- `The code segment selector.`
- `typeAttr` (`I64`)
- `Gate type and attribute byte.`

#### `function setGateWithIst( self, I64 vector, I64 handler, I64 selector, I64 typeAttr, I64 ist ) -> Void`

Install a handler with a dedicated stack index.

**Parameters**:

- `vector` (`I64`)
- `The vector number` (`0-255`)
- `handler` (`I64`)
- `The handler function address.`
- `selector` (`I64`)
- `The code segment selector.`
- `typeAttr` (`I64`)
- `Gate type and attribute byte.`
- `ist` (`I64`)
- `Stack index` (`1-7`)

#### `function getEntry( self, I64 vector ) -> IdtEntry`

Return the entry for the specified vector number.

**Parameters**:

- `vector` (`I64`)
- `The vector number` (`0-255`)

**Returns**: `IdtEntry` — The gate descriptor entry.

#### `function getPointer( self ) -> IdtPointer`

Construct a table pointer for loading into the CPU.

**Returns**: `IdtPointer` — A pointer with limit = (count * 16) - 1 and base = entry storage address.

#### `function destroy( self ) -> Void`

Free the underlying memory allocation for the vector table. 

## function `loadIdt`

Load the interrupt vector table into the CPU. On x86-64 this executes lidt. On AArch64 this writes VBAR_EL1.

**Parameters**:

- `ptr` (`IdtPointer`)
- `The table pointer containing the base address and size limit.`

### Methods

#### `function loadIdt( IdtPointer ptr ) -> Void`

Load the interrupt vector table into the CPU. On x86-64 this executes lidt. On AArch64 this writes VBAR_EL1.

**Parameters**:

- `ptr` (`IdtPointer`)
- `The table pointer containing the base address and size limit.`

