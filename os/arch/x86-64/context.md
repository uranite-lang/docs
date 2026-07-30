# uranite.os.arch.x86-64.context

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

Switch execution context by saving callee-saved registers and swapping the stack pointer. Pushes RBX, RBP, R12-R15, stores RSP at *fromStackPtr, loads RSP from *toStackPtr, then pops the registers. The function's own return consumes the entry address prepared by makeContext.

**Parameters**:

- `fromStackPtr` (`I64`)
- `Pointer to the slot where the current RSP is saved.`
- `toStackPtr` (`I64`)
- `Pointer to the slot from which the target RSP is loaded.`

### Methods

#### `function swapContext( I64 fromStackPtr, I64 toStackPtr ) -> Void`

Switch execution context by saving callee-saved registers and swapping the stack pointer. Pushes RBX, RBP, R12-R15, stores RSP at *fromStackPtr, loads RSP from *toStackPtr, then pops the registers. The function's own return consumes the entry address prepared by makeContext.

**Parameters**:

- `fromStackPtr` (`I64`)
- `Pointer to the slot where the current RSP is saved.`
- `toStackPtr` (`I64`)
- `Pointer to the slot from which the target RSP is loaded.`

## function `makeContext`

Prepare a new execution context on a stack region. Writes the entry function address and six zeroed callee-saved register slots, returning the initial RSP for use with swapContext.

**Parameters**:

- `stackBase` (`I64`)
- `Base address of the allocated stack memory region.`
- `stackSize` (`I64`)
- `Size in bytes of the stack memory region.`
- `entryFn` (`I64`)
- `Address of the function to run when the context is first resumed.`

**Returns**: — I64:
The initial RSP value for the new context.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function makeContext( I64 stackBase, I64 stackSize, I64 entryFn ) -> I64`

Prepare a new execution context on a stack region. Writes the entry function address and six zeroed callee-saved register slots, returning the initial RSP for use with swapContext.

**Parameters**:

- `stackBase` (`I64`)
- `Base address of the allocated stack memory region.`
- `stackSize` (`I64`)
- `Size in bytes of the stack memory region.`
- `entryFn` (`I64`)
- `Address of the function to run when the context is first resumed.`

**Returns**: — I64:
The initial RSP value for the new context.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

