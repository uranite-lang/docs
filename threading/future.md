# uranite.threading.future

## Table of Contents

- [Imports](#imports)
- [const `FUTURE_PENDING`](#const-future-pending)
- [const `FUTURE_RUNNING`](#const-future-running)
- [const `FUTURE_FINISHED`](#const-future-finished)
- [const `FUTURE_CANCELLED`](#const-future-cancelled)
- [const `FUTURE_ERROR`](#const-future-error)
- [enum `FutureState`](#enum-futurestate)
- [class `FutureBase`](#class-futurebase)
  - [`FutureBase()`](#FutureBase)
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
- [class `Future`](#class-future)
  - [`Future()`](#Future)
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
- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.threading.atomic`
  - `AtomicI64`
- `uranite.threading.errors`
  - `ThreadCancellationError`
  - `ThreadError`

## const `FUTURE_PENDING`

State value indicating the future has not yet started execution.

## const `FUTURE_RUNNING`

State value indicating the future's task is currently executing.

## const `FUTURE_FINISHED`

State value indicating the future has completed successfully with a result.

## const `FUTURE_CANCELLED`

State value indicating the future was cancelled before completion.

## const `FUTURE_ERROR`

State value indicating the future's task failed with an error.

## enum `FutureState`

Enumeration of all possible states a future can be in during its lifecycle, from initial creation through completion or failure.

## class `FutureBase`

Non-generic core of the future system. Manages the state machine, result storage, completion signaling, and blocking wait logic using raw I64 values. Future<R> wraps this class to provide compile-time type safety via monomorphized generics.

State transitions follow a linear progression: Pending -> Running -> Finished/Cancelled/Error. The completion event is signaled when the future reaches a terminal state (Finished, Cancelled, or Error).

### Fields

| Name | Type | Access |
|------|------|--------|
| `resultBuffer` | `Memory<I64>` | protect |
| `exceptionSlot` | `Memory<I64>` | protect |
| `state` | `AtomicI64` | protect |
| `completionEvent` | `Event` | protect |
| `scheduler` | `Scheduler` | protect |

### Methods

#### `function FutureBase( self, Scheduler scheduler ) -> Void`

Create a new FutureBase in the Pending state with empty result and exception buffers.

**Parameters**:

- `scheduler` (`Scheduler`)
- `The scheduler to use for cooperative blocking when a task`
- `waits for this future's completion.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setRunning( self ) -> Void`

Atomically transition the future from Pending to Running state. Only succeeds if the current state is Pending; otherwise this is a no-op (compare-exchange fails silently).

#### `function completeRaw( self, I64 value ) -> Void`

Complete the future successfully with a raw I64 result value. Stores the result, sets the state to Finished, and signals the completion event to wake any blocked waiters.

**Parameters**:

- `value` (`I64`)
- `The raw result value to store.`

#### `function failRaw( self, I64 exceptionPtr ) -> Void`

Fail the future with a raw exception pointer. Stores the exception address, sets the state to Error, and signals the completion event to wake any blocked waiters.

**Parameters**:

- `exceptionPtr` (`I64`)
- `Raw memory address of the exception object, or 0 if no`
- `specific exception is available.`

#### `function waitForCompletion( self ) -> Void`

Block the calling task until the future reaches a terminal state (Finished, Cancelled, or Error). Uses cooperative blocking through the scheduler when running inside a scheduled task, or falls back to a busy-wait spin loop with pause instructions when no task context is available.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getState( self ) -> I64`

Return the current state of the future as a raw integer constant.

**Returns**: `I64` — One of FUTURE_PENDING, FUTURE_RUNNING, FUTURE_FINISHED, FUTURE_CANCELLED, or FUTURE_ERROR.

#### `function getRawResult( self ) -> I64`

Return the raw I64 result value. Only meaningful after the future has reached the Finished state.

**Returns**: `I64` — The raw result value stored by completeRaw.

#### `function getRawException( self ) -> I64`

Return the raw exception pointer. Only meaningful after the future has reached the Error state.

**Returns**: `I64` — The raw exception pointer stored by failRaw.

#### `function isDone( self ) -> Boolean`

Check whether the future has reached a terminal state.

**Returns**: `Boolean` — True if the future is Finished, Cancelled, or in Error state.

#### `function cancel( self ) -> Boolean`

Attempt to cancel the future. Only succeeds if the future is still in the Pending state; a Running or completed future cannot be cancelled.

**Returns**: `Boolean` — True if the future was successfully cancelled, False if it had already progressed past the Pending state.

#### `function destroy( self ) -> Void`

Release all resources held by this future, including the result buffer, exception slot, and atomic state variable.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `Future`<R>

Type-safe generic wrapper around FutureBase that provides compile-time typed access to the result of an asynchronous task. The type parameter R specifies the return type of the underlying computation. Internally delegates all state management to the wrapped FutureBase.

### Fields

| Name | Type | Access |
|------|------|--------|
| `base` | `FutureBase` | protect |

### Methods

#### `function Future( self, FutureBase base ) -> Void`

Create a new typed future wrapping an existing FutureBase.

**Parameters**:

- `base` (`FutureBase`)
- `The non-generic future core to wrap.`

#### `function result( self ) -> R`

Block until the future completes and return the typed result. Reconstructs the value of type R from the raw I64 stored in the FutureBase by writing it into a temporary Memory<R> buffer and reading it back with proper type interpretation.

**Returns**: `R` — The result value produced by the completed task.

**Raises**:

- `ThreadError` → `Error` — If the task execution failed with an error.
- `ThreadCancellationError` → `ThreadError` → `Error` — If the future was cancelled before completion.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function wait( self ) -> Void`

Block until the future completes without retrieving the result. Useful when you need to ensure completion but do not need the return value.

**Raises**:

- `ThreadError` → `Error` — If the task execution failed with an error.
- `ThreadCancellationError` → `ThreadError` → `Error` — If the future was cancelled before completion.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function state( self ) -> FutureState`

Return the current state of the future as a FutureState enum value.

**Returns**: — FutureState:
The current lifecycle state of the future.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isDone( self ) -> Boolean`

Check whether the future has reached a terminal state.

**Returns**: `Boolean` — True if the future is Finished, Cancelled, or in Error state.

#### `function cancel( self ) -> Boolean`

Attempt to cancel the future. Only succeeds if the future is still in the Pending state.

**Returns**: `Boolean` — True if the future was successfully cancelled, False otherwise.

#### `function getBase( self ) -> FutureBase`

Return the underlying FutureBase instance for low-level access.

**Returns**: `FutureBase` — The non-generic future core wrapped by this instance.

#### `function destroy( self ) -> Void`

Release all resources held by the underlying FutureBase.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

