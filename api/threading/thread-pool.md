# uranite.threading.thread-pool

## Table of Contents

- [Imports](#imports)
- [const `POOL_TASK_PENDING`](#const-pool-task-pending)
- [const `POOL_TASK_RUNNING`](#const-pool-task-running)
- [const `POOL_TASK_COMPLETED`](#const-pool-task-completed)
- [const `POOL_TASK_FAILED`](#const-pool-task-failed)
- [class `PoolTask`](#class-pooltask)
  - [`PoolTask()`](#PoolTask)
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
- [class `PoolHandle`](#class-poolhandle)
  - [`PoolHandle()`](#PoolHandle)
  - [`awaitResult()`](#awaitResult)
  - [`isReady()`](#isReady)
- [class `ThreadPool`](#class-threadpool)
  - [`ThreadPool()`](#ThreadPool)
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
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.threading.atomic`
  - `AtomicBoolean`
  - `AtomicI64`
- `uranite.threading.channel`
  - `ChannelInner`
- `uranite.threading.errors`
  - `ThreadError`
- `uranite.threading.join-handle`
  - `JoinHandle`
- `uranite.threading.thread`
  - `Thread`
  - `ThreadRuntime`

## const `POOL_TASK_PENDING`

State value indicating the pool task has not yet started execution.

## const `POOL_TASK_RUNNING`

State value indicating the pool task is currently executing.

## const `POOL_TASK_COMPLETED`

State value indicating the pool task completed successfully.

## const `POOL_TASK_FAILED`

State value indicating the pool task failed with an error.

## class `PoolTask`<R, A>

Generic task wrapper for thread pool submission. Encapsulates a function pointer, a typed argument buffer, a typed result buffer, and lifecycle state tracking. The worker thread reads the argument, invokes the function, and stores the result or marks the task as failed.

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

#### `function PoolTask( self, I64 functionPtr, A argument ) -> Void`

Create a new pool task wrapping a function and its argument.

**Parameters**:

- `functionPtr` (`I64`)
- `Raw address of the function to execute.`
- `argument` (`A`)
- `The typed argument to pass to the function.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getFunctionPtr( self ) -> I64`

Return the raw function pointer address.

**Returns**: `I64` — The address of the function to execute.

#### `function getArgument( self ) -> A`

Return the typed argument stored in the argument buffer.

**Returns**: `A` — The argument value to pass to the function.

#### `function setResult( self, R value ) -> Void`

Store the result value and transition the task to the Completed state. Signals the completion event to wake any blocked waiters.

**Parameters**:

- `value` (`R`)
- `The result value produced by the function.`

#### `function getResult( self ) -> R`

Return the typed result value. Only meaningful after the task has reached the Completed state.

**Returns**: `R` — The result value stored by setResult.

#### `function markRunning( self ) -> Void`

Transition the task to the Running state, indicating that a worker has begun executing the function.

#### `function markFailed( self ) -> Void`

Mark the task as failed, set the error flag, and signal the completion event to wake any blocked waiters.

#### `function isCompleted( self ) -> Boolean`

Check whether the task has reached a terminal state.

**Returns**: `Boolean` — True if the task is Completed or Failed.

#### `function hasFailed( self ) -> Boolean`

Check whether the task failed with an error.

**Returns**: `Boolean` — True if the task was marked as failed.

#### `function getCompletionEvent( self ) -> Event`

Return the kernel event used for completion notification.

**Returns**: `Event` — The event that is signaled when the task completes or fails.

#### `function destroy( self ) -> Void`

Release all resources held by this task, including argument and result buffers and atomic state variables.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `PoolHandle`<R>

Handle for awaiting the typed result of a pool task. Provides a one-shot awaitResult method that blocks until the task completes and returns the typed result. Cleans up all task-related buffers after the result is consumed.

Each PoolHandle can only be awaited once -- calling awaitResult a second time raises a ThreadError.

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

#### `function PoolHandle( self, Memory<R> resultBuffer, Event completionEvent, AtomicI64 state, AtomicBoolean hasError, Memory<I64> argBuffer, Scheduler scheduler ) -> Void`

Create a new pool handle bound to the given task's internal state.

**Parameters**:

- `resultBuffer` (`Memory<R>`)
- `The task's result buffer.`
- `completionEvent` (`Event`)
- `The task's completion event.`
- `state` (`AtomicI64`)
- `The task's atomic state variable.`
- `hasError` (`AtomicBoolean`)
- `The task's error flag.`
- `argBuffer` (`Memory<I64>`)
- `The task's argument buffer.`
- `scheduler` (`Scheduler`)
- `The scheduler to use for cooperative blocking.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function awaitResult( self ) -> R`

Block until the task completes and return the typed result. Cleans up all task-related buffers and atomic variables after retrieving the result. This method can only be called once per handle.

**Returns**: `R` — The typed result value produced by the pool task.

**Raises**:

- `ThreadError` → `Error` — If this handle has already been awaited, or if the pool task failed during execution.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function isReady( self ) -> Boolean`

Check whether the associated task has reached a terminal state without blocking.

**Returns**: — Boolean:
True if the task is Completed or Failed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ThreadPool`

Low-level thread pool that accepts raw task pointers via a shared channel. Workers are pre-spawned threads that continuously receive and execute tasks from the work channel. Tracks active workers and total submissions via atomic counters.

For typed task submission with generic result handling, use PoolTask and PoolHandle on top of this pool, or prefer ThreadPoolExecutor for a higher-level API with built-in future support.

### Fields

| Name | Type | Access |
|------|------|--------|
| `workers` | `Memory<Thread>` | protect |
| `handles` | `Memory<JoinHandle>` | protect |
| `workerCount` | `I64` | protect |
| `workChannel` | `ChannelInner<I64>` | protect |
| `runtime` | `ThreadRuntime` | protect |
| `shutdown` | `AtomicBoolean` | protect |
| `activeWorkers` | `AtomicI64` | protect |
| `totalSubmitted` | `AtomicI64` | protect |

### Methods

#### `function ThreadPool( self, I64 numWorkers, I64 queueCapacity, ThreadRuntime runtime ) -> Void`

Create a new thread pool with the specified number of workers and queue capacity. Workers are not started automatically -- they must be configured and started externally.

**Parameters**:

- `numWorkers` (`I64`)
- `Number of worker threads to allocate storage for.`
- `queueCapacity` (`I64`)
- `Maximum number of pending tasks the work channel can hold.`
- `runtime` (`ThreadRuntime`)
- `The thread runtime providing scheduler and memory management.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function submitTaskPointer( self, I64 taskPointer ) -> Void`

Submit a raw task pointer to the pool's work channel for execution by a worker thread.

**Parameters**:

- `taskPointer` (`I64`)
- `Raw memory address of the task to execute.`

**Raises**:

- `ThreadError` → `Error` — If the pool has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function shutdownAndWait( self ) -> Void`

Initiate pool shutdown by setting the shutdown flag and closing the work channel. Workers will finish their current tasks and exit when they find the channel closed.

#### `function getWorkerCount( self ) -> I64`

Return the number of worker threads in the pool.

**Returns**: `I64` — The worker count configured at construction.

#### `function getTotalSubmitted( self ) -> I64`

Return the total number of tasks submitted to the pool.

**Returns**: `I64` — Cumulative count of submitted tasks.

#### `function isShutdown( self ) -> Boolean`

Check whether the pool has been shut down.

**Returns**: `Boolean` — True if shutdown has been initiated.

#### `function destroy( self ) -> Void`

Release all resources held by the pool, including worker and handle storage, the work channel, and all atomic counters.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

