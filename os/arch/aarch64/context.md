# uranite.os.arch.aarch64.context

## Table of Contents

- [Imports](#imports)
- [function `swapContext`](#function-swapcontext)
  - [`swapContext()`](#swapContext)
- [function `makeContext`](#function-makecontext)
  - [`makeContext()`](#makeContext)

## Imports

- `uranite.os.arch.native.memory`
  - `writeI64At`

## function `swapContext`

Switch execution context by saving the AArch64 callee-saved registers (X19-X28, X29/FP, X30/LR) onto the current stack, storing SP at *fromStackPtr, loading SP from *toStackPtr, restoring the registers, then returning via the restored X30. A freshly prepared context resumes at the entry address placed by makeContext in its X30 slot.

**Parameters**:

- `fromStackPtr` (`I64`)
- `Pointer to the slot where the current SP is saved.`
- `toStackPtr` (`I64`)
- `Pointer to the slot from which the target SP is loaded.`

### Methods

#### `function swapContext( I64 fromStackPtr, I64 toStackPtr ) -> Void`

Switch execution context by saving the AArch64 callee-saved registers (X19-X28, X29/FP, X30/LR) onto the current stack, storing SP at *fromStackPtr, loading SP from *toStackPtr, restoring the registers, then returning via the restored X30. A freshly prepared context resumes at the entry address placed by makeContext in its X30 slot.

**Parameters**:

- `fromStackPtr` (`I64`)
- `Pointer to the slot where the current SP is saved.`
- `toStackPtr` (`I64`)
- `Pointer to the slot from which the target SP is loaded.`

## function `makeContext`

Prepare a new execution context on a stack region. Lays out the twelve callee-saved register slots (X19-X28, X29, X30) with X30 set to the entry function and the rest zeroed, returning the initial SP for swapContext.

The frame is 96 bytes; slots are laid out at increasing offsets matching swapContext: SP+0..SP+80 hold X19..X29 and SP+88 holds X30 (the entry).

**Parameters**:

- `stackBase` (`I64`)
- `Base address of the allocated stack memory region.`
- `stackSize` (`I64`)
- `Size in bytes of the stack memory region.`
- `entryFn` (`I64`)
- `Address of the function to run when the context is first resumed.`

**Returns**: — I64:
The initial SP value (16-byte aligned) for the new context.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function makeContext( I64 stackBase, I64 stackSize, I64 entryFn ) -> I64`

Prepare a new execution context on a stack region. Lays out the twelve callee-saved register slots (X19-X28, X29, X30) with X30 set to the entry function and the rest zeroed, returning the initial SP for swapContext.

The frame is 96 bytes; slots are laid out at increasing offsets matching swapContext: SP+0..SP+80 hold X19..X29 and SP+88 holds X30 (the entry).

**Parameters**:

- `stackBase` (`I64`)
- `Base address of the allocated stack memory region.`
- `stackSize` (`I64`)
- `Size in bytes of the stack memory region.`
- `entryFn` (`I64`)
- `Address of the function to run when the context is first resumed.`

**Returns**: — I64:
The initial SP value (16-byte aligned) for the new context.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

