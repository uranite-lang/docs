# uranite.errors.panic

## Table of Contents

- [Imports](#imports)
- [class `PanicHandler`](#class-panichandler)
  - [`PanicHandler()`](#PanicHandler)
  - [`panic()`](#panic)
  - [`isPanicked()`](#isPanicked)
  - [`getReason()`](#getReason)
  - [`getFaultAddress()`](#getFaultAddress)
  - [`getFrameCount()`](#getFrameCount)
  - [`getFrame()`](#getFrame)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.debug.panic`
  - `PanicContext`
  - `PanicReason`

## class `PanicHandler`

Unrecoverable error handler with diagnostic context capture. Wraps the kernel PanicContext to capture register state, stack backtrace, and faulting address before halting the CPU. Use for fatal conditions where recovery is impossible.

### Fields

| Name | Type | Access |
|------|------|--------|
| `context` | `PanicContext` | protect |

### Methods

#### `function PanicHandler( self, I64 maxStackFrames ) -> Void`

Construct a panic handler with capacity for the given number of stack backtrace frames.

**Parameters**:

- `maxStackFrames` (`I64`)
- `Maximum number of stack frames to capture on panic.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function panic( self, I64 reason, I64 faultAddress ) -> Void`

Trigger an unrecoverable panic. Captures register state and stack backtrace, then halts the CPU. Does not return.

**Parameters**:

- `reason` (`I64`)
- `PanicReason backed value identifying the fault category.`
- `faultAddress` (`I64`)
- `Address associated with the fault` (`e.g., null pointer`)

#### `function isPanicked( self ) -> Boolean`

Return whether a panic has been triggered. 

#### `function getReason( self ) -> I64`

Return the panic reason code. 

#### `function getFaultAddress( self ) -> I64`

Return the faulting address from the panic. 

#### `function getFrameCount( self ) -> I64`

Return the number of stack frames captured. 

#### `function getFrame( self, I64 index ) -> I64`

Return stack frame address at the given backtrace index.

**Parameters**:

- `index` (`I64`)
- `Zero-based index into the captured stack frames.`

**Returns**: `I64` — Frame address, or 0 if index is out of range.

#### `function destroy( self ) -> Void`

Free all resources held by the panic context.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

