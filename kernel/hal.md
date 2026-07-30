# uranite.kernel.hal

## Table of Contents

- [Imports](#imports)
- [class `HardwareAbstraction`](#class-hardwareabstraction)
  - [`HardwareAbstraction()`](#HardwareAbstraction)
  - [`readRegister8()`](#readRegister8)
  - [`writeRegister8()`](#writeRegister8)
  - [`readRegister32()`](#readRegister32)
  - [`writeRegister32()`](#writeRegister32)
  - [`readRegister64()`](#readRegister64)
  - [`writeRegister64()`](#writeRegister64)
  - [`registerIrq()`](#registerIrq)
  - [`unregisterIrq()`](#unregisterIrq)

## Imports

- `uranite.os.hal.interrupt`
  - `InterruptTable`
  - `IrqEntry`
  - `MAX_IRQ_COUNT`
- `uranite.os.hal.mmio`
  - `loadFence`
  - `memoryFence`
  - `mmioRead16`
  - `mmioRead32`
  - `mmioRead64`
  - `mmioRead8`
  - `mmioWrite16`
  - `mmioWrite32`
  - `mmioWrite64`
  - `mmioWrite8`
  - `storeFence`

## class `HardwareAbstraction`

Unified HAL combining MMIO register access with automatic memory barrier enforcement and interrupt table management.

### Fields

| Name | Type | Access |
|------|------|--------|
| `interrupts` | `InterruptTable` | public |

### Methods

#### `function HardwareAbstraction( self ) -> Void`

#### `function readRegister8( self, I64 address ) -> I64`

Read an 8-bit MMIO register with a load fence. 

#### `function writeRegister8( self, I64 address, I64 value ) -> Void`

Write an 8-bit MMIO register with a store fence. 

#### `function readRegister32( self, I64 address ) -> I64`

Read a 32-bit MMIO register with a load fence. 

#### `function writeRegister32( self, I64 address, I64 value ) -> Void`

Write a 32-bit MMIO register with a store fence. 

#### `function readRegister64( self, I64 address ) -> I64`

Read a 64-bit MMIO register with a load fence. 

#### `function writeRegister64( self, I64 address, I64 value ) -> Void`

Write a 64-bit MMIO register with a store fence. 

#### `function registerIrq( self, I64 irqNumber, I64 handlerId ) -> Boolean`

Register an interrupt handler for the given IRQ number. 

#### `function unregisterIrq( self, I64 irqNumber ) -> I64`

Unregister the interrupt handler for the given IRQ number. Returns the previous handler address, or 0 if none was registered.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

