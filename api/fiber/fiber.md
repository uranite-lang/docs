# uranite.fiber.fiber

## Table of Contents

- [Imports](#imports)
- [class `FiberError`](#class-fibererror)
  - [`FiberError()`](#FiberError)
- [const `SYS_MMAP`](#const-sys-mmap)
- [const `SYS_MUNMAP`](#const-sys-munmap)
- [const `PROT_RW`](#const-prot-rw)
- [const `MAP_PRIVATE_ANON`](#const-map-private-anon)
- [const `DEFAULT_FIBER_STACK`](#const-default-fiber-stack)
- [const `FIBER_CREATED`](#const-fiber-created)
- [const `FIBER_STARTED`](#const-fiber-started)
- [const `FIBER_RUNNING`](#const-fiber-running)
- [const `FIBER_SUSPENDED`](#const-fiber-suspended)
- [const `FIBER_TERMINATED`](#const-fiber-terminated)
- [const `F_STATE`](#const-f-state)
- [const `F_ENTRY_FN`](#const-f-entry-fn)
- [const `F_STACK_BASE`](#const-f-stack-base)
- [const `F_STACK_SIZE`](#const-f-stack-size)
- [const `F_CONTEXT_RSP`](#const-f-context-rsp)
- [const `F_RETURN_VALUE`](#const-f-return-value)
- [const `F_SUSPEND_VALUE`](#const-f-suspend-value)
- [const `F_RESUME_VALUE`](#const-f-resume-value)
- [const `F_HAS_RETURN`](#const-f-has-return)
- [const `F_ERROR`](#const-f-error)
- [const `FIBER_FIELD_COUNT`](#const-fiber-field-count)
- [const `gCurrentFiberPtr`](#const-gcurrentfiberptr)
- [const `gCallerRspPtr`](#const-gcallerrspptr)
- [function `fiberTrampoline`](#function-fibertrampoline)
  - [`fiberTrampoline()`](#fiberTrampoline)
- [function `fiberSuspend`](#function-fibersuspend)
  - [`fiberSuspend()`](#fiberSuspend)
- [class `Fiber`](#class-fiber)
  - [`Fiber()`](#Fiber)
  - [`Fiber()`](#Fiber)
  - [`start()`](#start)
  - [`resume()`](#resume)
  - [`getReturn()`](#getReturn)
  - [`isStarted()`](#isStarted)
  - [`isRunning()`](#isRunning)
  - [`isSuspended()`](#isSuspended)
  - [`isTerminated()`](#isTerminated)
  - [`destroy()`](#destroy)

## Imports

- `uranite.async.context`
  - `makeContext`
  - `swapContext`
- `uranite.errors.error`
  - `Error`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readI64At`
  - `writeI64At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall2`
  - `syscall6`
- `uranite.os.syscall.result`
  - `SyscallResult`

## class `FiberError`

**Extends**: `Error`

Error raised when a fiber operation fails, such as attempting to start an already-started fiber, resuming a non-suspended fiber, or a stack allocation failure during fiber creation.

### Methods

#### `function FiberError( self, String message ) -> Void`

Create a new fiber error with the given message.

**Parameters**:

- `message` (`String`)
- `Description of the fiber operation that failed.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## const `SYS_MMAP`

Linux syscall number for mmap.

## const `SYS_MUNMAP`

Linux syscall number for munmap.

## const `PROT_RW`

Memory protection flags for read-write access (PROT_READ | PROT_WRITE).

## const `MAP_PRIVATE_ANON`

Mmap flags for private anonymous mapping (MAP_PRIVATE | MAP_ANONYMOUS).

## const `DEFAULT_FIBER_STACK`

Default stack size for fibers in bytes (64 KB).

## const `FIBER_CREATED`

Fiber state indicating it has been created but not yet started.

## const `FIBER_STARTED`

Fiber state indicating it has been started (transitional).

## const `FIBER_RUNNING`

Fiber state indicating it is currently executing.

## const `FIBER_SUSPENDED`

Fiber state indicating it has yielded and is waiting to be resumed.

## const `FIBER_TERMINATED`

Fiber state indicating it has finished execution.

## const `F_STATE`

Field offset index for the fiber's state in the data array.

## const `F_ENTRY_FN`

Field offset index for the fiber's entry function address.

## const `F_STACK_BASE`

Field offset index for the fiber's stack base address.

## const `F_STACK_SIZE`

Field offset index for the fiber's stack size.

## const `F_CONTEXT_RSP`

Field offset index for the fiber's saved stack pointer (RSP).

## const `F_RETURN_VALUE`

Field offset index for the fiber's return value.

## const `F_SUSPEND_VALUE`

Field offset index for the value passed out when suspending.

## const `F_RESUME_VALUE`

Field offset index for the value passed in when resuming.

## const `F_HAS_RETURN`

Field offset index for the flag indicating whether a return value was set.

## const `F_ERROR`

Field offset index for the fiber's error state.

## const `FIBER_FIELD_COUNT`

Total number of fields in the fiber data array.

## const `gCurrentFiberPtr`

Global pointer to the currently executing fiber's data array. Set before each context switch into a fiber and read by fiberTrampoline and fiberSuspend.

## const `gCallerRspPtr`

Global pointer to the caller's saved RSP buffer. Used to return control from the fiber back to the caller (scheduler or direct invoker).

## function `fiberTrampoline`

Entry point trampoline for newly started fibers. Called on the fiber's own stack after the initial context switch. Reads the entry function address from the fiber's data, invokes it via inline assembly indirect call, stores the return value, marks the fiber as terminated, and swaps context back to the caller.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fiberTrampoline(  ) -> Void`

Entry point trampoline for newly started fibers. Called on the fiber's own stack after the initial context switch. Reads the entry function address from the fiber's data, invokes it via inline assembly indirect call, stores the return value, marks the fiber as terminated, and swaps context back to the caller.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fiberSuspend`

Suspend the currently executing fiber, yielding control back to the caller. Stores the given value in the fiber's suspend slot so the caller can read it, then performs a context switch. When the fiber is later resumed, returns the resume value that was passed in.

**Parameters**:

- `value` (`I64`)
- `The value to yield to the caller. The caller receives this`
- `from start`

**Returns**: — I64:
The value passed by the caller when resume() is called.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function fiberSuspend( I64 value ) -> I64`

Suspend the currently executing fiber, yielding control back to the caller. Stores the given value in the fiber's suspend slot so the caller can read it, then performs a context switch. When the fiber is later resumed, returns the resume value that was passed in.

**Parameters**:

- `value` (`I64`)
- `The value to yield to the caller. The caller receives this`
- `from start`

**Returns**: — I64:
The value passed by the caller when resume() is called.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Fiber`

Cooperative lightweight thread that runs on its own mmap-allocated stack. Fibers execute a single entry function, can suspend and resume with bidirectional value passing, and must be explicitly destroyed to free their stack memory.

The fiber lifecycle is: Created -> Running (via start) -> Suspended (via fiberSuspend) -> Running (via resume) -> ... -> Terminated. Values can be passed in both directions: the caller sends values through resume(value), and the fiber sends values through fiberSuspend(value).
 Uses x86_64 context switching (callee-saved register save/restore + RSP swap) for zero-overhead cooperative multitasking.

### Fields

| Name | Type | Access |
|------|------|--------|
| `data` | `Memory<I64>` | public |
| `callerRsp` | `Memory<I64>` | public |

### Methods

#### `function Fiber( self, <type> entryFn ) -> Void`

Create a new fiber that will execute the given callable. Allocates a 64 KB stack via mmap and prepares the initial context so that the first context switch jumps to fiberTrampoline.

**Parameters**:

- `entryFn` (`Callable<I64, <>>`)
- `The entry function to execute. Receives no arguments and`
- `its return value is stored as the fiber's return value.`

**Raises**:

- `FiberError` → `Error` — If the stack allocation mmap syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function Fiber( self, I64 entryFn ) -> Void`

Create a new fiber from a raw function address. Prefer the Callable overload for type-safe construction.

**Parameters**:

- `entryFn` (`I64`)
- `Raw address of the entry function.`

**Raises**:

- `FiberError` → `Error` — If the stack allocation mmap syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function initFiber( self, I64 entryFnAddr ) -> Void`

Initialize fiber state, allocate stack, and prepare the initial execution context.

**Parameters**:

- `entryFnAddr` (`I64`)
- `Raw address of the entry function.`

**Raises**:

- `FiberError` → `Error` — If the stack allocation mmap syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function start( self ) -> I64`

Start the fiber's execution for the first time. Performs a context switch from the caller to the fiber's stack, where fiberTrampoline invokes the entry function. Returns when the fiber suspends or terminates.

**Returns**: `I64` — The suspend value yielded by the fiber via fiberSuspend, or 0 if the fiber terminated without suspending.

**Raises**:

- `FiberError` → `Error` — If the fiber has already been started.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function resume( self, I64 value ) -> I64`

Resume a suspended fiber, passing a value that becomes the return value of the fiber's fiberSuspend call. Returns when the fiber suspends again or terminates.

**Parameters**:

- `value` (`I64`)
- `The value to send to the fiber. The fiber receives this as`
- `the return value of fiberSuspend.`

**Returns**: `I64` — The next suspend value yielded by the fiber, or 0 if the fiber terminated.

**Raises**:

- `FiberError` → `Error` — If the fiber is not in the Suspended state.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getReturn( self ) -> I64`

Retrieve the return value of a terminated fiber.

**Returns**: `I64` — The value returned by the fiber's entry function.

**Raises**:

- `FiberError` → `Error` — If the fiber has not terminated or did not produce a return value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isStarted( self ) -> Boolean`

Check whether the fiber has been started.

**Returns**: `Boolean` — True if the fiber has progressed past the Created state.

#### `function isRunning( self ) -> Boolean`

Check whether the fiber is currently executing.

**Returns**: `Boolean` — True if the fiber is in the Running state.

#### `function isSuspended( self ) -> Boolean`

Check whether the fiber is suspended and waiting to be resumed.

**Returns**: `Boolean` — True if the fiber is in the Suspended state.

#### `function isTerminated( self ) -> Boolean`

Check whether the fiber has finished execution.

**Returns**: `Boolean` — True if the fiber is in the Terminated state.

#### `function destroy( self ) -> Void`

Release all resources held by the fiber. Unmaps the stack memory via munmap syscall and frees the data and callerRsp buffers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

