# uranite.os.debug.panic

## Table of Contents

- [Imports](#imports)
- [enum `PanicReason`](#enum-panicreason)
- [class `PanicContext`](#class-paniccontext)
  - [`PanicContext()`](#PanicContext)
  - [`panic()`](#panic)
  - [`getReason()`](#getReason)
  - [`getAddress()`](#getAddress)
  - [`getFrameCount()`](#getFrameCount)
  - [`getFrame()`](#getFrame)
  - [`isPanicked()`](#isPanicked)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## enum `PanicReason`

Kernel panic reason codes identifying the category of fatal error that triggered the panic. Used by PanicContext to record the cause of a crash.

## class `PanicContext`

Captures the full crash context when a kernel panic occurs, including the panic reason code, faulting address, CPU register state (stack pointer and instruction pointer), and a stack backtrace obtained by walking the frame pointer chain. After capturing this context, the CPU is halted.

### Fields

| Name | Type | Access |
|------|------|--------|
| `reason` | `I64` | public |
| `address` | `I64` | public |
| `stackPointer` | `I64` | public |
| `instructionPointer` | `I64` | public |
| `stackFrames` | `Memory<I64>` | public |
| `frameCount` | `I64` | public |
| `maxFrames` | `I64` | public |
| `hasPanicked` | `I64` | public |

### Methods

#### `function PanicContext( self, I64 maxStackFrames ) -> Void`

Construct a panic context with capacity for the specified maximum number of stack frames. All fields are initialized to zero and the stack frame buffer is allocated and zeroed. The hasPanicked flag remains 0 until panic() is called.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function panic( self, I64 reason, I64 addr ) -> Void`

Trigger a kernel panic with the given reason code and faulting address. Captures the current stack pointer (RSP), instruction pointer (RIP), and base pointer (RBP) using inline assembly, then walks the frame pointer chain to collect a stack backtrace. After capturing all context, disables interrupts (CLI) and halts the CPU (HLT).

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unwindStack( self, I64 framePointer ) -> Void`

Walk the frame pointer chain starting from the given base pointer to collect return addresses into the stack frames buffer. Stops when a null frame pointer is encountered or the maximum frame count is reached. Each frame pointer in the chain points to the previous frame's base pointer on the stack.

#### `function getReason( self ) -> I64`

Return the panic reason code that triggered this panic context. 

#### `function getAddress( self ) -> I64`

Return the faulting address associated with this panic. 

#### `function getFrameCount( self ) -> I64`

Return the number of stack frames captured during the panic. 

#### `function getFrame( self, I64 index ) -> I64`

Return the stack frame address at the given index in the backtrace. Returns 0 if the index is out of range.

#### `function isPanicked( self ) -> Boolean`

Return whether a panic has been triggered in this context. 

#### `function destroy( self ) -> Void`

Free the stack frames memory buffer.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

