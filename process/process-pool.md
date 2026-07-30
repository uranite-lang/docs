# uranite.process.process-pool

## Table of Contents

- [Imports](#imports)
- [const `POOL_PROC_PENDING`](#const-pool-proc-pending)
- [const `POOL_PROC_RUNNING`](#const-pool-proc-running)
- [const `POOL_PROC_COMPLETED`](#const-pool-proc-completed)
- [const `POOL_PROC_FAILED`](#const-pool-proc-failed)
- [class `ProcessPoolTask`](#class-processpooltask)
  - [`ProcessPoolTask()`](#ProcessPoolTask)
  - [`getFunctionPtr()`](#getFunctionPtr)
  - [`getArgument()`](#getArgument)
  - [`setResult()`](#setResult)
  - [`getResult()`](#getResult)
  - [`markRunning()`](#markRunning)
  - [`markFailed()`](#markFailed)
  - [`isCompleted()`](#isCompleted)
  - [`hasFailed()`](#hasFailed)
  - [`getCompletionEvent()`](#getCompletionEvent)
  - [`destroy()`](#destroy)
- [class `ProcessPoolHandle`](#class-processpoolhandle)
  - [`ProcessPoolHandle()`](#ProcessPoolHandle)
  - [`awaitResult()`](#awaitResult)
  - [`isReady()`](#isReady)
- [class `ProcessPool`](#class-processpool)
  - [`ProcessPool()`](#ProcessPool)
  - [`submitTaskPointer()`](#submitTaskPointer)
  - [`shutdownAndWait()`](#shutdownAndWait)
  - [`getWorkerCount()`](#getWorkerCount)
  - [`getTotalSubmitted()`](#getTotalSubmitted)
  - [`isShutdown()`](#isShutdown)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.memory.pmm`
  - `PhysicalMemoryManager`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.os.time.clock`
  - `SystemClock`
- `uranite.process.errors`
  - `ProcessError`
  - `ProcessPoolError`
- `uranite.process.process`
  - `Process`
  - `ProcessRuntime`
- `uranite.process.process-handle`
  - `ProcessHandle`
- `uranite.threading.atomic`
  - `AtomicBoolean`
  - `AtomicI64`
- `uranite.threading.channel`
  - `ChannelInner`

## const `POOL_PROC_PENDING`

Pool task state: queued but not yet picked up by a worker. 

## const `POOL_PROC_RUNNING`

Pool task state: currently being executed by a worker process. 

## const `POOL_PROC_COMPLETED`

Pool task state: execution completed successfully. 

## const `POOL_PROC_FAILED`

Pool task state: execution failed with an error. 

## class `ProcessPoolTask`<R, A>

A typed task submitted to a process pool for execution.

Wraps a function pointer together with a typed argument buffer and a typed result buffer. Tracks execution state via an atomic state machine and signals completion through an Event for cooperative blocking.

### Fields

| Name | Type | Access |
|------|------|--------|
| `functionPtr` | `I64` | protect |
| `argBuffer` | `Memory<A>` | protect |
| `resultBuffer` | `Memory<R>` | protect |
| `completionEvent` | `Event` | protect |
| `state` | `AtomicI64` | protect |
| `hasError` | `AtomicBoolean` | protect |

### Methods

#### `function ProcessPoolTask( self, I64 functionPtr, A argument ) -> Void`

Construct a new pool task with the given function and argument.

**Parameters**:

- `functionPtr` (`I64`)
- `Raw address of the function to execute.`
- `argument` (`A`)
- `The typed argument to pass to the function.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getFunctionPtr( self ) -> I64`

Return the raw function pointer address for this task.

**Returns**: `I64` — The function address to execute.

#### `function getArgument( self ) -> A`

Return the typed argument stored in the argument buffer.

**Returns**: `A` — The argument value to pass to the function.

#### `function setResult( self, R value ) -> Void`

Store the typed result and transition to completed state.

Writes the result value, sets the state to POOL_PROC_COMPLETED, and signals the completion event to wake any waiting tasks.

**Parameters**:

- `value` (`R`)
- `The result value produced by the function.`

#### `function getResult( self ) -> R`

Return the typed result value.

Should only be called after the task has completed successfully.

**Returns**: `R` — The result value stored by setResult.

#### `function markRunning( self ) -> Void`

Transition the task to the running state.

Called by a worker process when it begins executing this task.

#### `function markFailed( self ) -> Void`

Mark the task as failed and signal waiters.

Sets the error flag, transitions to POOL_PROC_FAILED state, and signals the completion event.

#### `function isCompleted( self ) -> Boolean`

Check whether the task has reached a terminal state.

**Returns**: `Boolean` — True if the task is completed or failed.

#### `function hasFailed( self ) -> Boolean`

Check whether the task failed during execution.

**Returns**: `Boolean` — True if the task execution encountered an error.

#### `function getCompletionEvent( self ) -> Event`

Return the event used for completion signaling.

**Returns**: `Event` — The event that is signaled when the task finishes or fails.

#### `function destroy( self ) -> Void`

Release all resources held by this task.

Frees the argument buffer, result buffer, and destroys the atomic state and error flag. Must be called after the result has been retrieved or the task is no longer needed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ProcessPoolHandle`<R>

Handle for awaiting a typed result from a process pool task.

Provides a one-shot await mechanism that blocks until the corresponding pool task completes, then returns the typed result. The handle takes ownership of the task's buffers and atomics, freeing them after the result is retrieved or on failure.

### Fields

| Name | Type | Access |
|------|------|--------|
| `taskResultBuffer` | `Memory<R>` | protect |
| `taskCompletionEvent` | `Event` | protect |
| `taskState` | `AtomicI64` | protect |
| `taskHasError` | `AtomicBoolean` | protect |
| `taskArgBuffer` | `Memory<I64>` | protect |
| `awaited` | `Boolean` | protect |
| `scheduler` | `Scheduler` | protect |

### Methods

#### `function ProcessPoolHandle( self, Memory<R> resultBuffer, Event completionEvent, AtomicI64 state, AtomicBoolean hasError, Memory<I64> argBuffer, Scheduler scheduler ) -> Void`

Construct a new pool handle from the task's internal components.

**Parameters**:

- `resultBuffer` (`Memory<R>`)
- `The task's result buffer.`
- `completionEvent` (`Event`)
- `The task's completion event for blocking.`
- `state` (`AtomicI64`)
- `The task's atomic state.`
- `hasError` (`AtomicBoolean`)
- `The task's error flag.`
- `argBuffer` (`Memory<I64>`)
- `The task's argument buffer to be freed on await.`
- `scheduler` (`Scheduler`)
- `The scheduler for cooperative blocking.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function awaitResult( self ) -> R`

Block until the pool task completes and return the typed result.

Can only be called once per handle. Blocks cooperatively via the scheduler if running in a scheduled context, or spin-waits otherwise. Frees the task's buffers and atomics before returning or raising.

**Returns**: `R` — The typed result produced by the pool task.

**Raises**:

- `ProcessError` → `Error` — If the handle has already been awaited, or if the task failed.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function isReady( self ) -> Boolean`

Check whether the pool task has reached a terminal state without blocking.

**Returns**: — Boolean:
True if the task is completed or failed, False if still running.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ProcessPool`

Pool of worker processes that execute tasks submitted via a shared channel.

Manages a fixed number of worker processes backed by a ProcessRuntime. Tasks are submitted as raw pointers through a bounded channel and distributed to workers. Supports shutdown with optional drain and tracks submission statistics.

### Fields

| Name | Type | Access |
|------|------|--------|
| `workers` | `Memory<Process>` | protect |
| `handles` | `Memory<ProcessHandle>` | protect |
| `workerCount` | `I64` | protect |
| `workChannel` | `ChannelInner<I64>` | protect |
| `runtime` | `ProcessRuntime` | protect |
| `shutdown` | `AtomicBoolean` | protect |
| `activeWorkers` | `AtomicI64` | protect |
| `totalSubmitted` | `AtomicI64` | protect |

### Methods

#### `function ProcessPool( self, I64 numWorkers, I64 queueCapacity, ?ProcessRuntime runtime ) -> Void`

Construct a new process pool with the specified number of workers.

If no runtime is provided, creates a default one with a new scheduler, a 1 MiB physical memory manager, a 1000-tick system clock, and a maximum of 256 processes.

**Parameters**:

- `numWorkers` (`I64`)
- `Number of worker processes to create. Must be at least 1.`
- `queueCapacity` (`I64`)
- `Maximum number of pending tasks the work channel can hold.`
- `runtime` (`ProcessRuntime`)
- `Optional pre-configured runtime, or None for a default one.`

**Raises**:

- `ProcessPoolError` → `ProcessError` → `Error` — If numWorkers is less than or equal to 0.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function submitTaskPointer( self, I64 taskPointer ) -> Void`

Submit a raw task pointer to the pool for execution by a worker.

The task pointer is sent through the work channel and will be picked up by the next available worker process.

**Parameters**:

- `taskPointer` (`I64`)
- `Raw pointer to the task descriptor, cast to I64.`

**Raises**:

- `ProcessError` → `Error` — If the pool has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function shutdownAndWait( self ) -> Void`

Shut down the pool and wait for all pending tasks to complete.

Sets the shutdown flag to prevent new submissions and closes the work channel's receive end to signal workers to exit.

#### `function getWorkerCount( self ) -> I64`

Return the number of worker processes in the pool.

**Returns**: `I64` — The fixed worker count configured at construction.

#### `function getTotalSubmitted( self ) -> I64`

Return the total number of tasks submitted to the pool.

**Returns**: `I64` — The cumulative count of all submitted tasks.

#### `function isShutdown( self ) -> Boolean`

Check whether the pool has been shut down.

**Returns**: `Boolean` — True if shutdown has been called, False otherwise.

#### `function destroy( self ) -> Void`

Release all resources held by the pool.

Frees worker and handle arrays, destroys the work channel, and tears down all atomic counters. Must be called after shutdown.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

