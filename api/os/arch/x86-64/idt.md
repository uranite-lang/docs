# uranite.os.arch.x86-64.idt

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

Single 16-byte interrupt gate descriptor for the x86_64 Interrupt Descriptor Table in long mode. Each entry maps an interrupt vector number (0-255) to a handler function address, a code segment selector, gate type attributes, and an optional Interrupt Stack Table index. The handler address is split across three fields (offsetLow, offsetMiddle, offsetHigh) due to the historical evolution of the x86 descriptor format from 16-bit to 64-bit mode.

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

Construct a zeroed IDT entry with no handler installed. All fields are initialized to zero, which means the entry is marked as not-present and will trigger a General Protection Fault if the corresponding interrupt vector fires before a handler is configured via setHandler.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setHandler( self, I64 handler, I64 selector, I64 typeAttr, I64 ist ) -> Void`

Configure this IDT entry with an interrupt handler address, code segment selector, gate type and attributes, and an Interrupt Stack Table index. The 64-bit handler address is split into three parts to match the hardware descriptor format. The selector should point to a kernel code segment. The typeAttr byte encodes the gate type (interrupt or trap), DPL (ring level required to invoke via software int), and the present bit. The IST field (1-7) selects a dedicated stack from the TSS for this handler, or 0 to use the current stack.

**Parameters**:

- `handler` (`I64`)
- `The 64-bit virtual address of the interrupt handler function.`
- `selector` (`I64`)
- `The GDT code segment selector to load into CS on handler entry.`
- `typeAttr` (`I64`)
- `The gate type and attribute byte` (`interrupt gate, trap gate, DPL`)
- `ist` (`I64`)
- `The IST index` (`1-7`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getHandler( self ) -> I64`

Reconstruct and return the full 64-bit handler address by combining the three offset fields (offsetLow, offsetMiddle, offsetHigh) back into a single contiguous virtual address.

**Returns**: — I64:
The reconstructed 64-bit handler function address.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## struct `IdtPointer`

Descriptor for the IDTR register, loaded by the lidt instruction. Contains a 2-byte limit field specifying the IDT size minus one in bytes, and an 8-byte linear base address pointing to the first IDT entry in memory. The processor uses this structure to locate interrupt gate descriptors when an interrupt or exception fires.

### Fields

| Name | Type | Access |
|------|------|--------|
| `limit` | `I64` | public |
| `base` | `I64` | public |

### Methods

#### `function IdtPointer( self, I64 limit, I64 base ) -> Void`

Construct an IDT pointer with the given size limit and base address. The limit should be set to the total IDT size in bytes minus one (typically 256 * 16 - 1 = 4095 for a full IDT), and the base should point to the linear address of the first IdtEntry in memory.

**Parameters**:

- `limit` (`I64`)
- `The IDT size in bytes minus one.`
- `base` (`I64`)
- `The linear address of the first IDT entry.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## enum `GateType`

Gate type attribute bytes for x86_64 IDT entries, encoding the gate type, DPL (privilege level), and present bit into a single byte value.

## enum `InterruptVector`

CPU exception and interrupt vector numbers for x86_64. Vectors 0-31 are reserved by the processor for hardware exceptions. Vectors 32-255 are available for hardware IRQs and software-defined interrupts.

## const `IRQ_BASE`

Base interrupt vector number where hardware IRQs start after PIC remapping. 

## class `Idt`

Full Interrupt Descriptor Table containing 256 gate descriptor entries, covering all possible interrupt vector numbers on x86_64. Vectors 0-31 are reserved by the processor for CPU exceptions (divide error, page fault, general protection fault, etc.). Vectors 32-255 are available for hardware IRQs (typically starting at vector 32 after PIC remapping) and software- defined interrupts including system calls.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<IdtEntry>` | protect |
| `count` | `I64` | protect |

### Methods

#### `function Idt( self ) -> Void`

Construct a full IDT with 256 zeroed gate descriptor entries. All entries start as not-present, meaning any interrupt vector that fires without a registered handler will trigger a General Protection Fault. Handlers must be installed via setGate or setGateWithIst before enabling interrupts.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function setGate( self, I64 vector, I64 handler, I64 selector, I64 typeAttr ) -> Void`

Install an interrupt handler for the specified vector number using IST 0, which means the handler runs on the current stack. The vector must be in the range 0-255. The handler address, code segment selector, and gate type attributes are written into the corresponding IDT entry.

**Parameters**:

- `vector` (`I64`)
- `The interrupt vector number` (`0-255`)
- `handler` (`I64`)
- `The virtual address of the interrupt handler function.`
- `selector` (`I64`)
- `The GDT code segment selector to load into CS on entry.`
- `typeAttr` (`I64`)
- `The gate type and attribute byte` (`interrupt gate, trap gate, DPL`)

#### `function setGateWithIst( self, I64 vector, I64 handler, I64 selector, I64 typeAttr, I64 ist ) -> Void`

Install an interrupt handler with a dedicated Interrupt Stack Table entry for automatic stack switching on entry. IST indices 1-7 reference stacks defined in the Task State Segment. This is critical for handlers that must not rely on the current stack being valid, such as double fault (IST 1) and NMI (IST 2), where the original stack may be corrupted or exhausted.

**Parameters**:

- `vector` (`I64`)
- `The interrupt vector number` (`0-255`)
- `handler` (`I64`)
- `The virtual address of the interrupt handler function.`
- `selector` (`I64`)
- `The GDT code segment selector to load into CS on entry.`
- `typeAttr` (`I64`)
- `The gate type and attribute byte` (`interrupt gate, trap gate, DPL`)
- `ist` (`I64`)
- `The IST index` (`1-7`)

#### `function getEntry( self, I64 vector ) -> IdtEntry`

Return the IDT entry for the specified interrupt vector number. The returned IdtEntry can be inspected to check the installed handler address, gate type, and IST configuration.

**Parameters**:

- `vector` (`I64`)
- `The interrupt vector number` (`0-255`)

**Returns**: `IdtEntry` — The gate descriptor entry for the given vector.

#### `function destroy( self ) -> Void`

Free the underlying memory allocation used to store all 256 IDT entries. This should only be called when the IDT is no longer loaded into the IDTR, as freeing an active IDT will cause undefined behavior on the next interrupt.

## function `loadIdt`

Load the Interrupt Descriptor Table into the IDTR register using the lidt instruction. The IdtPointer provides the base address and size limit of the IDT to the processor. After loading, the processor will use this IDT to dispatch all interrupts and exceptions to their registered handlers.

**Parameters**:

- `ptr` (`IdtPointer`)
- `The IDT pointer containing the base address and size limit.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function loadIdt( IdtPointer ptr ) -> Void`

Load the Interrupt Descriptor Table into the IDTR register using the lidt instruction. The IdtPointer provides the base address and size limit of the IDT to the processor. After loading, the processor will use this IDT to dispatch all interrupts and exceptions to their registered handlers.

**Parameters**:

- `ptr` (`IdtPointer`)
- `The IDT pointer containing the base address and size limit.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

