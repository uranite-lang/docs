# uranite.fiber.fiber-scheduler

## Table of Contents

- [Imports](#imports)
- [const `MAX_FIBERS`](#const-max-fibers)
- [class `FiberScheduler`](#class-fiberscheduler)
  - [`FiberScheduler()`](#FiberScheduler)
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
- `uranite.coroutine.scheduler`
  - `Scheduler`
- `uranite.fiber.fiber`
  - `FIBER_CREATED`
  - `FIBER_RUNNING`
  - `FIBER_SUSPENDED`
  - `FIBER_TERMINATED`
  - `F_CONTEXT_RSP`
  - `F_STATE`
  - `Fiber`
  - `FiberError`
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

## const `MAX_FIBERS`

Maximum number of fibers that can be managed by a single scheduler.

## class `FiberScheduler`

**Implements**: `Scheduler`

Cooperative scheduler for lightweight fibers. Manages a fixed-size pool of fiber slots and round-robin schedules fibers that are in the Created or Suspended state. Each call to runOnce picks the next eligible fiber and context-switches into it. The run method loops until all fibers have terminated.

The scheduler maintains its own main RSP buffer so that fibers can return control to the scheduler's context after suspending or terminating.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fiberSlots` | `Memory<I64>` | public |
| `fiberCount` | `I64` | public |
| `nextFiberId` | `I64` | public |
| `currentIdx` | `I64` | public |
| `mainRspBuf` | `Memory<I64>` | public |

### Methods

#### `function FiberScheduler( self ) -> Void`

Create a new fiber scheduler with empty fiber slots and capacity for up to MAX_FIBERS concurrent fibers.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function spawn( self, <type> entryFn ) -> I64`

Create a new fiber running the given callable and register it in the scheduler's slot array.

**Parameters**:

- `entryFn` (`Callable<I64, <>>`)
- `The entry function for the fiber.`

**Returns**: `I64` — A unique fiber identifier assigned to the new fiber.

**Raises**:

- `FiberError` → `Error` — If the maximum fiber count has been reached or no free slot is available.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function spawn( self, I64 entryFn ) -> I64`

Create a new fiber from a raw function address and register it in the scheduler's slot array. The fiber is created in the Created state and will be picked up by the next runOnce call.

**Parameters**:

- `entryFn` (`I64`)
- `Raw address of the entry function for the fiber.`

**Returns**: `I64` — A unique fiber identifier assigned to the new fiber.

**Raises**:

- `FiberError` → `Error` — If the maximum fiber count has been reached or no free slot is available.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function yieldCurrent( self ) -> Void`

Yield the currently executing fiber back to the scheduler. If no fiber is currently running (currentIdx < 0), this is a no-op. The fiber transitions to the Suspended state and control returns to the scheduler.

#### `function hasPending( self ) -> Boolean`

Check whether any fibers are still eligible for execution.

**Returns**: — Boolean:
True if at least one fiber is in the Created or Suspended
state, False if all fibers have terminated or no fibers
are registered.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function runOnce( self ) -> Void`

Find the next eligible fiber (Created or Suspended) and context-switch into it. The fiber runs until it suspends or terminates, at which point control returns to this method. If no eligible fiber is found, returns immediately without switching.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function run( self ) -> Void`

Run all registered fibers to completion. Repeatedly calls runOnce in a loop until no fibers remain in the Created or Suspended state, then performs cleanup.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function cleanup( self ) -> Void`

Clear all fiber slots and reset the fiber count to zero. Does not free the fiber stack memory -- individual fibers must be destroyed separately if their stacks need to be reclaimed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release all resources held by the scheduler. Performs cleanup of fiber slots, then frees the slot array and main RSP buffer.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

