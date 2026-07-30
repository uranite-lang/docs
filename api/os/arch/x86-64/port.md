# uranite.os.arch.x86-64.port

## Table of Contents

- [function `outb`](#function-outb)
  - [`outb()`](#outb)
- [function `inb`](#function-inb)
  - [`inb()`](#inb)
- [function `outw`](#function-outw)
  - [`outw()`](#outw)
- [function `inw`](#function-inw)
  - [`inw()`](#inw)
- [function `outl`](#function-outl)
  - [`outl()`](#outl)
- [function `inl`](#function-inl)
  - [`inl()`](#inl)
- [function `ioWait`](#function-iowait)
  - [`ioWait()`](#ioWait)

## function `outb`

Write a single byte value to the specified x86 I/O port address using the outb instruction. The x86 architecture provides a separate 64KB I/O address space distinct from main memory, accessed through dedicated in/out instructions. Legacy hardware devices such as the Programmable Interrupt Controller (PIC), Programmable Interval Timer (PIT), serial UART controllers, PS/2 keyboard controller, and PCI configuration space all communicate through this I/O port mechanism rather than memory-mapped registers.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)
- `value` (`I64`)
- `The byte value to write` (`only the low 8 bits are used`)

### Methods

#### `function outb( I64 port, I64 value ) -> Void`

Write a single byte value to the specified x86 I/O port address using the outb instruction. The x86 architecture provides a separate 64KB I/O address space distinct from main memory, accessed through dedicated in/out instructions. Legacy hardware devices such as the Programmable Interrupt Controller (PIC), Programmable Interval Timer (PIT), serial UART controllers, PS/2 keyboard controller, and PCI configuration space all communicate through this I/O port mechanism rather than memory-mapped registers.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)
- `value` (`I64`)
- `The byte value to write` (`only the low 8 bits are used`)

## function `inb`

Read a single byte from the specified x86 I/O port address using the inb instruction. The result is returned in the accumulator register (AL) and zero-extended to a full I64 value. This is the most common I/O port width used by legacy PC hardware for status registers, data registers, and control registers.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)

**Returns**: `I64` — The byte value read from the port, zero-extended to I64.

### Methods

#### `function inb( I64 port ) -> I64`

Read a single byte from the specified x86 I/O port address using the inb instruction. The result is returned in the accumulator register (AL) and zero-extended to a full I64 value. This is the most common I/O port width used by legacy PC hardware for status registers, data registers, and control registers.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)

**Returns**: `I64` — The byte value read from the port, zero-extended to I64.

## function `outw`

Write a 16-bit word value to the specified x86 I/O port address using the outw instruction. Some hardware devices use 16-bit I/O port access for transferring larger values in a single operation, such as certain PCI configuration registers and some VGA registers.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)
- `value` (`I64`)
- `The word value to write` (`only the low 16 bits are used`)

### Methods

#### `function outw( I64 port, I64 value ) -> Void`

Write a 16-bit word value to the specified x86 I/O port address using the outw instruction. Some hardware devices use 16-bit I/O port access for transferring larger values in a single operation, such as certain PCI configuration registers and some VGA registers.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)
- `value` (`I64`)
- `The word value to write` (`only the low 16 bits are used`)

## function `inw`

Read a 16-bit word from the specified x86 I/O port address using the inw instruction. The result is returned in AX and zero-extended to I64. Used for hardware that requires 16-bit port access width.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)

**Returns**: `I64` — The 16-bit word value read from the port, zero-extended to I64.

### Methods

#### `function inw( I64 port ) -> I64`

Read a 16-bit word from the specified x86 I/O port address using the inw instruction. The result is returned in AX and zero-extended to I64. Used for hardware that requires 16-bit port access width.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)

**Returns**: `I64` — The 16-bit word value read from the port, zero-extended to I64.

## function `outl`

Write a 32-bit doubleword value to the specified x86 I/O port address using the outl instruction. 32-bit port access is commonly used by PCI configuration space reads and writes, where the configuration register address is written to port 0xCF8 and data is transferred through port 0xCFC.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)
- `value` (`I64`)
- `The doubleword value to write` (`only the low 32 bits are used`)

### Methods

#### `function outl( I64 port, I64 value ) -> Void`

Write a 32-bit doubleword value to the specified x86 I/O port address using the outl instruction. 32-bit port access is commonly used by PCI configuration space reads and writes, where the configuration register address is written to port 0xCF8 and data is transferred through port 0xCFC.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)
- `value` (`I64`)
- `The doubleword value to write` (`only the low 32 bits are used`)

## function `inl`

Read a 32-bit doubleword from the specified x86 I/O port address using the inl instruction. The result is returned in EAX and zero-extended to I64. Primarily used for PCI configuration space access where 32-bit register reads are standard.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)

**Returns**: `I64` — The 32-bit doubleword value read from the port, zero-extended to I64.

### Methods

#### `function inl( I64 port ) -> I64`

Read a 32-bit doubleword from the specified x86 I/O port address using the inl instruction. The result is returned in EAX and zero-extended to I64. Primarily used for PCI configuration space access where 32-bit register reads are standard.

**Parameters**:

- `port` (`I64`)
- `The I/O port address` (`0-65535`)

**Returns**: `I64` — The 32-bit doubleword value read from the port, zero-extended to I64.

## function `ioWait`

Insert a short delay of approximately one microsecond by performing a dummy write to I/O port 0x80, which is the POST diagnostic code port on IBM PC compatible hardware. This delay is required between consecutive I/O port operations targeting slow devices such as the Programmable Interrupt Controller and Programmable Interval Timer, which need time to process each command before accepting the next one.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function ioWait(  ) -> Void`

Insert a short delay of approximately one microsecond by performing a dummy write to I/O port 0x80, which is the POST diagnostic code port on IBM PC compatible hardware. This delay is required between consecutive I/O port operations targeting slow devices such as the Programmable Interrupt Controller and Programmable Interval Timer, which need time to process each command before accepting the next one.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

