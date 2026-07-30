# uranite.os.hal.interrupt

## Table of Contents

- [Imports](#imports)
- [const `MAX_IRQ_COUNT`](#const-max-irq-count)
- [class `IrqEntry`](#class-irqentry)
  - [`IrqEntry()`](#IrqEntry)
  - [`isRegistered()`](#isRegistered)
  - [`incrementTrigger()`](#incrementTrigger)
- [class `InterruptTable`](#class-interrupttable)
  - [`InterruptTable()`](#InterruptTable)
  - [`registerHandler()`](#registerHandler)
  - [`unregisterHandler()`](#unregisterHandler)
  - [`getHandler()`](#getHandler)
  - [`mask()`](#mask)
  - [`unmask()`](#unmask)
  - [`isEnabled()`](#isEnabled)
  - [`isRegistered()`](#isRegistered)
  - [`recordTrigger()`](#recordTrigger)
  - [`getTriggerCount()`](#getTriggerCount)
  - [`getRegisteredCount()`](#getRegisteredCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## const `MAX_IRQ_COUNT`

The maximum number of supported IRQ lines, covering legacy PIC and IOAPIC. 

## class `IrqEntry`

Represents a single IRQ handler entry storing the handler function address, IRQ number, enabled state, and trigger count for tracking how many times the interrupt has fired.

### Fields

| Name | Type | Access |
|------|------|--------|
| `handlerAddress` | `I64` | public |
| `irqNumber` | `I64` | public |
| `enabled` | `Boolean` | public |
| `triggerCount` | `I64` | public |

### Methods

#### `function IrqEntry( self ) -> Void`

Construct an IRQ entry with no handler registered, disabled, and zero trigger count.

#### `function isRegistered( self ) -> Boolean`

Return whether a handler function has been registered for this IRQ. 

#### `function incrementTrigger( self ) -> Void`

Increment the trigger count by one, recording that the interrupt has fired.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `InterruptTable`

Interrupt routing table that maps up to 256 IRQ numbers to handler function addresses. Supports registering, unregistering, masking (disabling), and unmasking (enabling) individual IRQ lines. Each IRQ also tracks its trigger count for diagnostic purposes. All registration and unregistration operations are protected by a spinlock for interrupt-safe concurrent access.

### Fields

| Name | Type | Access |
|------|------|--------|
| `handlers` | `Memory<I64>` | protect |
| `enabled` | `Memory<I64>` | protect |
| `triggerCounts` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `registeredCount` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function InterruptTable( self ) -> Void`

Construct an interrupt table with capacity for 256 IRQ lines. All handlers are initialized to null (0), all IRQs are disabled, and all trigger counts are zeroed.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function registerHandler( self, I64 irq, I64 handlerAddress ) -> Boolean`

Register an interrupt handler at the given function address for the specified IRQ number. The IRQ is automatically enabled after registration. Returns True on success, False if the IRQ number is out of range.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function unregisterHandler( self, I64 irq ) -> I64`

Unregister the interrupt handler for the specified IRQ number. Returns the previously registered handler address, or 0 if no handler was registered or the IRQ number is out of range.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getHandler( self, I64 irq ) -> I64`

Return the handler address for the given IRQ number, or 0 if none is registered or the IRQ is out of range. 

#### `function mask( self, I64 irq ) -> Boolean`

Mask (disable) the specified IRQ, stopping delivery of interrupts without removing the registered handler. Returns True on success, False if the IRQ number is out of range.

#### `function unmask( self, I64 irq ) -> Boolean`

Unmask (enable) the specified IRQ, resuming delivery of interrupts. Only succeeds if a handler is registered for the IRQ. Returns True on success, False if the IRQ is out of range or has no handler.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isEnabled( self, I64 irq ) -> Boolean`

Return whether the specified IRQ is currently enabled (unmasked). 

#### `function isRegistered( self, I64 irq ) -> Boolean`

Return whether a handler has been registered for the specified IRQ. 

#### `function recordTrigger( self, I64 irq ) -> Void`

Record that the specified IRQ has fired by incrementing its trigger count. This is called from the interrupt service routine dispatcher for diagnostic and performance monitoring purposes.

#### `function getTriggerCount( self, I64 irq ) -> I64`

Return the number of times the specified IRQ has been triggered, or 0 if out of range. 

#### `function getRegisteredCount( self ) -> I64`

Return the total number of registered interrupt handlers. 

#### `function destroy( self ) -> Void`

Free all Memory arrays used for storing handler, enabled, and trigger count data.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

