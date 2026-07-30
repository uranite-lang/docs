# uranite.coroutine.scheduler

## Table of Contents

- [Imports](#imports)
- [class `SchedulerError`](#class-schedulererror)
  - [`SchedulerError()`](#SchedulerError)
- [const `MAX_COROUTINES`](#const-max-coroutines)
- [interface `Scheduler`](#interface-scheduler)
  - [`spawn()`](#spawn)
  - [`run()`](#run)
  - [`hasPending()`](#hasPending)
- [class `CoroutineScheduler`](#class-coroutinescheduler)
  - [`CoroutineScheduler()`](#CoroutineScheduler)
  - [`spawn()`](#spawn)
  - [`spawn()`](#spawn)
  - [`yieldCurrent()`](#yieldCurrent)
  - [`hasPending()`](#hasPending)
  - [`runOnce()`](#runOnce)
  - [`run()`](#run)
  - [`cleanup()`](#cleanup)
  - [`destroy()`](#destroy)

## Imports

- `uranite.async.context`
  - `swapContext`
- `uranite.errors.error`
  - `Error`
- `uranite.fiber.fiber`
  - `FIBER_CREATED`
  - `FIBER_RUNNING`
  - `FIBER_SUSPENDED`
  - `FIBER_TERMINATED`
  - `F_CONTEXT_RSP`
  - `F_STATE`
  - `Fiber`
  - `fiberSuspend`
  - `fiberTrampoline`
  - `gCallerRspPtr`
  - `gCurrentFiberPtr`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readI64At`
  - `writeI64At`
- `uranite.memory.memory`
  - `Memory`

## class `SchedulerError`

**Extends**: `Error`

### Methods

#### `function SchedulerError( self, String message ) -> Void`

## const `MAX_COROUTINES`

## interface `Scheduler`

### Methods

#### `function spawn( self, I64 entryFn ) -> I64`

Spawn new coroutine, return ID. 

#### `function run( self ) -> Void`

Run event loop until all coroutines complete. 

#### `function hasPending( self ) -> Boolean`

Check for pending coroutines.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `CoroutineScheduler`

**Implements**: `Scheduler`

### Fields

| Name | Type | Access |
|------|------|--------|
| `slots` | `Memory<I64>` | public |
| `count` | `I64` | public |
| `nextId` | `I64` | public |
| `currentIdx` | `I64` | public |
| `mainRspBuf` | `Memory<I64>` | public |

### Methods

#### `function CoroutineScheduler( self ) -> Void`

#### `function spawn( self, <type> entryFn ) -> I64`

Spawn a new coroutine running the given callable.

**Parameters**:

- `entryFn` (`Callable<I64, <>>`)
- `The coroutine entry function.`

**Returns**: `I64` — A unique coroutine identifier.

**Raises**:

- `SchedulerError` → `Error` — If the maximum coroutine count has been reached.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function spawn( self, I64 entryFn ) -> I64`

#### `function yieldCurrent( self ) -> Void`

#### `function hasPending( self ) -> Boolean`

#### `function runOnce( self ) -> Void`

#### `function run( self ) -> Void`

#### `function cleanup( self ) -> Void`

#### `function destroy( self ) -> Void`

