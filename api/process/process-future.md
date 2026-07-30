# uranite.process.process-future

## Table of Contents

- [Imports](#imports)
- [const `PFUTURE_PENDING`](#const-pfuture-pending)
- [const `PFUTURE_RUNNING`](#const-pfuture-running)
- [const `PFUTURE_FINISHED`](#const-pfuture-finished)
- [const `PFUTURE_CANCELLED`](#const-pfuture-cancelled)
- [const `PFUTURE_ERROR`](#const-pfuture-error)
- [enum `ProcessFutureState`](#enum-processfuturestate)
- [class `ProcessFutureBase`](#class-processfuturebase)
  - [`ProcessFutureBase()`](#ProcessFutureBase)
  - [`setRunning()`](#setRunning)
  - [`completeRaw()`](#completeRaw)
  - [`failRaw()`](#failRaw)
  - [`waitForCompletion()`](#waitForCompletion)
  - [`getState()`](#getState)
  - [`getRawResult()`](#getRawResult)
  - [`getRawException()`](#getRawException)
  - [`isDone()`](#isDone)
  - [`cancel()`](#cancel)
  - [`destroy()`](#destroy)
- [class `ProcessFuture`](#class-processfuture)
  - [`ProcessFuture()`](#ProcessFuture)
  - [`result()`](#result)
  - [`wait()`](#wait)
  - [`state()`](#state)
  - [`isDone()`](#isDone)
  - [`cancel()`](#cancel)
  - [`getBase()`](#getBase)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.ipc.shared-memory`
  - `SharedRegion`
- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.process.errors`
  - `ProcessCancellationError`
  - `ProcessCrashedError`
  - `ProcessError`
- `uranite.threading.atomic`
  - `AtomicI64`

## const `PFUTURE_PENDING`

Future state: task has not yet been picked up by a worker. 

## const `PFUTURE_RUNNING`

Future state: task is currently being executed by a worker. 

## const `PFUTURE_FINISHED`

Future state: task completed successfully with a result value. 

## const `PFUTURE_CANCELLED`

Future state: task was cancelled before execution began. 

## const `PFUTURE_ERROR`

Future state: task execution failed with an error. 

## enum `ProcessFutureState`

## class `ProcessFutureBase`

Non-generic core of the process future system.

Manages the state machine (pending, running, finished, cancelled, error), blocking semantics via scheduler integration, and raw I64 result/exception storage in a shared memory region. ProcessFuture<R> wraps this class to provide compile-time type safety.

### Fields

| Name | Type | Access |
|------|------|--------|
| `resultBuffer` | `Memory<I64>` | protect |
| `exceptionSlot` | `Memory<I64>` | protect |
| `state` | `AtomicI64` | protect |
| `completionEvent` | `Event` | protect |
| `scheduler` | `Scheduler` | protect |
| `region` | `SharedRegion` | protect |

### Methods

#### `function ProcessFutureBase( self, Scheduler scheduler, SharedRegion region, I64 slotAddr ) -> Void`

Construct a new ProcessFutureBase with storage at the given shared memory slot.

The result buffer is placed at slotAddr and the exception slot at slotAddr + 8, both within the provided shared memory region. The region's reference count is incremented to prevent premature release.

**Parameters**:

- `scheduler` (`Scheduler`)
- `The scheduler used for cooperative blocking on completion.`
- `region` (`SharedRegion`)
- `The shared memory region containing the result and exception slots.`
- `slotAddr` (`I64`)
- `The byte offset within the region for the result buffer.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setRunning( self ) -> Void`

Atomically transition the future from pending to running state.

Uses compare-and-exchange so only the first call succeeds. Subsequent calls are no-ops if the state has already advanced past pending.

#### `function completeRaw( self, I64 value ) -> Void`

Complete the future successfully with a raw I64 result value.

Stores the value in the result buffer, transitions to finished state, and signals all waiters via the completion event.

**Parameters**:

- `value` (`I64`)
- `The raw result value to store.`

#### `function failRaw( self, I64 exceptionPtr ) -> Void`

Fail the future with a raw I64 exception pointer.

Stores the exception pointer in the exception slot, transitions to error state, and signals all waiters via the completion event.

**Parameters**:

- `exceptionPtr` (`I64`)
- `Raw pointer to the exception object, cast to I64.`

#### `function waitForCompletion( self ) -> Void`

Block the current task until the future reaches a terminal state.

If running within a scheduled task, registers a wait on the completion event and cooperatively blocks via the scheduler. If running outside a scheduled context, spin-waits with pause instructions.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getState( self ) -> I64`

Return the current state of this future as a raw integer.

**Returns**: `I64` — One of PFUTURE_PENDING, PFUTURE_RUNNING, PFUTURE_FINISHED, PFUTURE_CANCELLED, or PFUTURE_ERROR.

#### `function getRawResult( self ) -> I64`

Return the raw I64 result value stored in the result buffer.

The caller must ensure the future has completed successfully before calling this method. Reading the result of an incomplete or failed future yields undefined data.

**Returns**: `I64` — The raw result value.

#### `function getRawException( self ) -> I64`

Return the raw I64 exception pointer stored in the exception slot.

The caller must ensure the future is in the error state before calling this method.

**Returns**: `I64` — The raw exception pointer, or 0 if no exception was stored.

#### `function isDone( self ) -> Boolean`

Check whether the future has reached a terminal state.

A future is done when it is finished, cancelled, or in error state.

**Returns**: `Boolean` — True if the future is in a terminal state, False otherwise.

#### `function cancel( self ) -> Boolean`

Attempt to cancel the future before execution begins.

Only succeeds if the future is still in the pending state. Once a worker has picked up the task (running state or later), cancellation is not possible.

**Returns**: `Boolean` — True if the future was successfully cancelled, False if it had already advanced past the pending state.

#### `function destroy( self ) -> Void`

Release all resources held by this future.

Destroys the atomic state and releases the reference on the shared memory region. Must be called exactly once after the future is no longer needed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ProcessFuture`<R>

Type-safe wrapper around ProcessFutureBase for process task results.

Provides compile-time type safety via the generic parameter R. Handles blocking, state inspection, cancellation, and typed result extraction by delegating to the underlying ProcessFutureBase.

### Fields

| Name | Type | Access |
|------|------|--------|
| `base` | `ProcessFutureBase` | protect |

### Methods

#### `function ProcessFuture( self, ProcessFutureBase base ) -> Void`

Construct a typed ProcessFuture wrapping the given base future.

**Parameters**:

- `base` (`ProcessFutureBase`)
- `The non-generic future to wrap with type safety.`

#### `function result( self ) -> R`

Block until the future completes and return the typed result.

Waits for the underlying task to finish, then reinterprets the raw I64 result as type R. Raises ProcessError if the task failed or ProcessCancellationError if the future was cancelled.

**Returns**: `R` — The result value produced by the process task.

**Raises**:

- `ProcessError` → `Error` — If the process task execution failed.
- `ProcessCancellationError` → `ProcessError` → `Error` — If the future was cancelled before execution.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function wait( self ) -> Void`

Block until the future completes, discarding the result.

Useful when the caller only needs to synchronize on completion without retrieving the value. Raises on failure or cancellation.

**Raises**:

- `ProcessError` → `Error` — If the process task execution failed.
- `ProcessCancellationError` → `ProcessError` → `Error` — If the future was cancelled before execution.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function state( self ) -> ProcessFutureState`

Return the current state of this future as a ProcessFutureState enum.

**Returns**: — ProcessFutureState:
The current lifecycle state of the future.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isDone( self ) -> Boolean`

Check whether the future has reached a terminal state.

**Returns**: `Boolean` — True if the future is finished, cancelled, or in error state.

#### `function cancel( self ) -> Boolean`

Attempt to cancel the future before a worker begins execution.

**Returns**: `Boolean` — True if cancellation succeeded, False if the task had already started or completed.

#### `function getBase( self ) -> ProcessFutureBase`

Return the underlying non-generic ProcessFutureBase.

**Returns**: `ProcessFutureBase` — The base future wrapped by this typed future.

#### `function destroy( self ) -> Void`

Release all resources held by this future and its base.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

