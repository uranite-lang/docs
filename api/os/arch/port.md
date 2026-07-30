# uranite.os.arch.port

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

Read an 8-bit value from the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)

**Returns**: `I64` — The 8-bit value read (zero-extended).

### Methods

#### `function inb( I64 port ) -> I64`

Read an 8-bit value from the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)

**Returns**: `I64` — The 8-bit value read (zero-extended).

## function `outb`

Write an 8-bit value to the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)
- `value` (`I64`)
- `The 8-bit value to write.`

### Methods

#### `function outb( I64 port, I64 value ) -> Void`

Write an 8-bit value to the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)
- `value` (`I64`)
- `The 8-bit value to write.`

## function `inw`

Read a 16-bit value from the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)

**Returns**: `I64` — The 16-bit value read (zero-extended).

### Methods

#### `function inw( I64 port ) -> I64`

Read a 16-bit value from the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)

**Returns**: `I64` — The 16-bit value read (zero-extended).

## function `outw`

Write a 16-bit value to the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)
- `value` (`I64`)
- `The 16-bit value to write.`

### Methods

#### `function outw( I64 port, I64 value ) -> Void`

Write a 16-bit value to the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)
- `value` (`I64`)
- `The 16-bit value to write.`

## function `inl`

Read a 32-bit value from the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)

**Returns**: `I64` — The 32-bit value read (zero-extended).

### Methods

#### `function inl( I64 port ) -> I64`

Read a 32-bit value from the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)

**Returns**: `I64` — The 32-bit value read (zero-extended).

## function `outl`

Write a 32-bit value to the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)
- `value` (`I64`)
- `The 32-bit value to write.`

### Methods

#### `function outl( I64 port, I64 value ) -> Void`

Write a 32-bit value to the given port/address.

**Parameters**:

- `port` (`I64`)
- `The I/O port` (`x86-64`)
- `value` (`I64`)
- `The 32-bit value to write.`

## function `ioWait`

I/O synchronization delay. On x86-64 this writes to the POST diagnostic port (0x80). On AArch64 this executes a DSB barrier.

### Methods

#### `function ioWait(  ) -> Void`

I/O synchronization delay. On x86-64 this writes to the POST diagnostic port (0x80). On AArch64 this executes a DSB barrier.

