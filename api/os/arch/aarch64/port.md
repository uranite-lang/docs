# uranite.os.arch.aarch64.port

## Table of Contents

- [function `inb`](#function-inb)
  - [`inb()`](#inb)
- [function `outb`](#function-outb)
  - [`outb()`](#outb)
- [function `inw`](#function-inw)
  - [`inw()`](#inw)
- [function `outw`](#function-outw)
  - [`outw()`](#outw)
- [function `inl`](#function-inl)
  - [`inl()`](#inl)
- [function `outl`](#function-outl)
  - [`outl()`](#outl)
- [function `ioWait`](#function-iowait)
  - [`ioWait()`](#ioWait)

## function `inb`

Read an 8-bit value from the given port address via MMIO. On AArch64, port I/O is emulated through memory-mapped access. The port number is treated as an offset from a platform-specific MMIO base.

**Parameters**:

- `port` (`I64`)
- `The port address to read from.`

**Returns**: `I64` — The 8-bit value read (zero-extended to I64).

### Methods

#### `function inb( I64 port ) -> I64`

Read an 8-bit value from the given port address via MMIO. On AArch64, port I/O is emulated through memory-mapped access. The port number is treated as an offset from a platform-specific MMIO base.

**Parameters**:

- `port` (`I64`)
- `The port address to read from.`

**Returns**: `I64` — The 8-bit value read (zero-extended to I64).

## function `outb`

Write an 8-bit value to the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to write to.`
- `value` (`I64`)
- `The 8-bit value to write` (`low byte used`)

### Methods

#### `function outb( I64 port, I64 value ) -> Void`

Write an 8-bit value to the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to write to.`
- `value` (`I64`)
- `The 8-bit value to write` (`low byte used`)

## function `inw`

Read a 16-bit value from the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to read from.`

**Returns**: `I64` — The 16-bit value read (zero-extended to I64).

### Methods

#### `function inw( I64 port ) -> I64`

Read a 16-bit value from the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to read from.`

**Returns**: `I64` — The 16-bit value read (zero-extended to I64).

## function `outw`

Write a 16-bit value to the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to write to.`
- `value` (`I64`)
- `The 16-bit value to write` (`low halfword used`)

### Methods

#### `function outw( I64 port, I64 value ) -> Void`

Write a 16-bit value to the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to write to.`
- `value` (`I64`)
- `The 16-bit value to write` (`low halfword used`)

## function `inl`

Read a 32-bit value from the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to read from.`

**Returns**: `I64` — The 32-bit value read (zero-extended to I64).

### Methods

#### `function inl( I64 port ) -> I64`

Read a 32-bit value from the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to read from.`

**Returns**: `I64` — The 32-bit value read (zero-extended to I64).

## function `outl`

Write a 32-bit value to the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to write to.`
- `value` (`I64`)
- `The 32-bit value to write` (`low word used`)

### Methods

#### `function outl( I64 port, I64 value ) -> Void`

Write a 32-bit value to the given port address via MMIO.

**Parameters**:

- `port` (`I64`)
- `The port address to write to.`
- `value` (`I64`)
- `The 32-bit value to write` (`low word used`)

## function `ioWait`

I/O wait for device synchronization. On x86-64 this writes to port 0x80 (POST diagnostic port) as a timing delay. On AArch64, a DSB instruction ensures all prior memory operations complete before proceeding.

### Methods

#### `function ioWait(  ) -> Void`

I/O wait for device synchronization. On x86-64 this writes to port 0x80 (POST diagnostic port) as a timing delay. On AArch64, a DSB instruction ensures all prior memory operations complete before proceeding.

