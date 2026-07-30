# uranite.process.executor

## Table of Contents

- [Imports](#imports)
- [const `DEFAULT_PROCESS_QUEUE_CAPACITY`](#const-default-process-queue-capacity)
- [const `EXECUTOR_PROCESS_STACK_PAGES`](#const-executor-process-stack-pages)
- [const `FUTURE_SLOT_SIZE`](#const-future-slot-size)
- [const `FUTURE_POOL_PAGES`](#const-future-pool-pages)
- [function `processWorkerLoop`](#function-processworkerloop)
  - [`processWorkerLoop()`](#processWorkerLoop)
- [class `ProcessPoolExecutor`](#class-processpoolexecutor)
  - [`ProcessPoolExecutor()`](#ProcessPoolExecutor)
  - [`shutdown()`](#shutdown)
  - [`isShutdown()`](#isShutdown)
  - [`getActiveCount()`](#getActiveCount)
  - [`getCompletedCount()`](#getCompletedCount)
  - [`getWorkerCount()`](#getWorkerCount)
  - [`destroy()`](#destroy)
- [class `ProcessSubmitter0`](#class-processsubmitter0)
  - [`ProcessSubmitter0()`](#ProcessSubmitter0)
  - [`submit()`](#submit)
- [class `ProcessSubmitter1`](#class-processsubmitter1)
  - [`ProcessSubmitter1()`](#ProcessSubmitter1)
  - [`submit()`](#submit)
- [class `ProcessSubmitter2`](#class-processsubmitter2)
  - [`ProcessSubmitter2()`](#ProcessSubmitter2)
  - [`submit()`](#submit)
- [class `ProcessSubmitter3`](#class-processsubmitter3)
  - [`ProcessSubmitter3()`](#ProcessSubmitter3)
  - [`submit()`](#submit)

## Imports

- `uranite.language.callable`
  - `Callable`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.ipc.shared-memory`
  - `SharedRegion`
- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.memory.pmm`
  - `PhysicalMemoryManager`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.task.task`
  - `BlockReason`
  - `Task`
- `uranite.os.time.clock`
  - `SystemClock`
- `uranite.process.errors`
  - `ProcessError`
- `uranite.process.process`
  - `DEFAULT_PROCESS_PRIORITY`
  - `DEFAULT_PROCESS_STACK_PAGES`
  - `Process`
  - `ProcessRuntime`
- `uranite.process.process-future`
  - `PFUTURE_ERROR`
  - `PFUTURE_FINISHED`
  - `PFUTURE_PENDING`
  - `PFUTURE_RUNNING`
  - `ProcessFuture`
  - `ProcessFutureBase`
- `uranite.threading.atomic`
  - `AtomicBoolean`
  - `AtomicI64`
- `uranite.threading.channel`
  - `ChannelInner`

## const `DEFAULT_PROCESS_QUEUE_CAPACITY`

Default maximum number of pending tasks in the executor work channel. 

## const `EXECUTOR_PROCESS_STACK_PAGES`

Number of stack pages allocated for each executor worker process. 

## const `FUTURE_SLOT_SIZE`

Size in bytes of each future slot in the shared memory pool (8 bytes result + 8 bytes exception). 

## const `FUTURE_POOL_PAGES`

Minimum number of pages for the future slot shared memory pool. 

## function `processWorkerLoop`

Main loop executed by each worker process in the executor pool.

Receives task descriptors from the shared channel, dispatches function calls via inline assembly indirect call based on arity (0-3 arguments), and completes or fails the associated ProcessFutureBase. Exits when the channel is closed or a zero descriptor address is received.

**Parameters**:

- `processId` (`I64`)
- `The unique identifier of this worker process.`
- `channelAddr` (`I64`)
- `Raw pointer to the ChannelInner<I64> for receiving task descriptors.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function processWorkerLoop( I64 processId, I64 channelAddr ) -> Void`

Main loop executed by each worker process in the executor pool.

Receives task descriptors from the shared channel, dispatches function calls via inline assembly indirect call based on arity (0-3 arguments), and completes or fails the associated ProcessFutureBase. Exits when the channel is closed or a zero descriptor address is received.

**Parameters**:

- `processId` (`I64`)
- `The unique identifier of this worker process.`
- `channelAddr` (`I64`)
- `Raw pointer to the ChannelInner<I64> for receiving task descriptors.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `ProcessPoolExecutor`

High-level process pool executor with future-based result retrieval.

Manages a fixed pool of worker processes that execute submitted functions in isolated address spaces. Workers dispatch calls via inline assembly indirect call through a shared channel. Results are stored in a shared memory future pool and accessed through ProcessFutureBase instances. Supports functions with 0-3 arguments via the run, submit1, submit2, and submit3 methods.

### Fields

| Name | Type | Access |
|------|------|--------|
| `workers` | `Memory<Process>` | protect |
| `workerCount` | `I64` | protect |
| `taskChannel` | `ChannelInner<I64>` | protect |
| `runtime` | `ProcessRuntime` | protect |
| `futurePoolRegion` | `SharedRegion` | protect |
| `nextFutureSlot` | `AtomicI64` | protect |
| `futurePoolCapacity` | `I64` | protect |
| `activeCount` | `AtomicI64` | protect |
| `completedCount` | `AtomicI64` | protect |
| `shutdownFlag` | `AtomicBoolean` | protect |

### Methods

#### `function ProcessPoolExecutor( self, I64 numWorkers, ?ProcessRuntime runtime ) -> Void`

Construct a new executor with the specified number of worker processes.

If no runtime is provided, creates a default one with a new scheduler, a 1 MiB physical memory manager, a 1000-tick system clock, and a maximum of 256 processes. Spawns worker processes immediately and enqueues them on the scheduler.

**Parameters**:

- `numWorkers` (`I64`)
- `Number of worker processes to create. Must be at least 1.`
- `runtime` (`ProcessRuntime`)
- `Optional pre-configured runtime, or None for a default one.`

**Raises**:

- `ProcessError` → `Error` — If numWorkers is less than or equal to 0.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function allocateFutureSlot( self ) -> I64`

Allocate the next available future slot from the shared memory pool.

Uses atomic round-robin allocation. The slot wraps around when the pool capacity is exceeded, so callers must ensure slots are consumed before they are reused.

**Returns**: `I64` — The physical address of the allocated future slot.

#### `function run( self, I64 fnAddr ) -> ProcessFutureBase`

Submit a zero-argument function for execution and return a future.

Builds a task descriptor with arity 0, allocates a future slot, and sends the descriptor through the task channel to a worker.

**Parameters**:

- `fnAddr` (`I64`)
- `Raw address of the function to execute.`

**Returns**: `ProcessFutureBase` — A future that will hold the result of the function call.

**Raises**:

- `ProcessError` → `Error` — If the executor has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function submit1( self, I64 fnAddr, I64 arg ) -> ProcessFutureBase`

Submit a one-argument function for execution and return a future.

Builds a task descriptor with arity 1, packing the argument at descriptor slot 3.

**Parameters**:

- `fnAddr` (`I64`)
- `Raw address of the function to execute.`
- `arg` (`I64`)
- `The single argument to pass to the function.`

**Returns**: `ProcessFutureBase` — A future that will hold the result of the function call.

**Raises**:

- `ProcessError` → `Error` — If the executor has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function submit2( self, I64 fnAddr, I64 arg1, I64 arg2 ) -> ProcessFutureBase`

Submit a two-argument function for execution and return a future.

Builds a task descriptor with arity 2, packing arguments at descriptor slots 3 and 4.

**Parameters**:

- `fnAddr` (`I64`)
- `Raw address of the function to execute.`
- `arg1` (`I64`)
- `The first argument to pass to the function.`
- `arg2` (`I64`)
- `The second argument to pass to the function.`

**Returns**: `ProcessFutureBase` — A future that will hold the result of the function call.

**Raises**:

- `ProcessError` → `Error` — If the executor has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function submit3( self, I64 fnAddr, I64 arg1, I64 arg2, I64 arg3 ) -> ProcessFutureBase`

Submit a three-argument function for execution and return a future.

Builds a task descriptor with arity 3, packing arguments at descriptor slots 3, 4, and 5.

**Parameters**:

- `fnAddr` (`I64`)
- `Raw address of the function to execute.`
- `arg1` (`I64`)
- `The first argument to pass to the function.`
- `arg2` (`I64`)
- `The second argument to pass to the function.`
- `arg3` (`I64`)
- `The third argument to pass to the function.`

**Returns**: `ProcessFutureBase` — A future that will hold the result of the function call.

**Raises**:

- `ProcessError` → `Error` — If the executor has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function shutdown( self, Boolean wait ) -> Void`

Shut down the executor, optionally waiting for workers to finish.

Sets the shutdown flag to reject new submissions and closes the task channel. If wait is True, spin-waits until every worker process has finished.

**Parameters**:

- `wait` (`Boolean`)
- `If True, blocks until all worker processes have terminated.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function isShutdown( self ) -> Boolean`

Check whether the executor has been shut down.

**Returns**: `Boolean` — True if shutdown has been called, False otherwise.

#### `function getActiveCount( self ) -> I64`

Return the number of tasks currently being executed.

**Returns**: `I64` — The count of active (submitted but not completed) tasks.

#### `function getCompletedCount( self ) -> I64`

Return the number of tasks that have completed execution.

**Returns**: `I64` — The cumulative count of completed tasks.

#### `function getWorkerCount( self ) -> I64`

Return the number of worker processes in the executor.

**Returns**: `I64` — The fixed worker count configured at construction.

#### `function destroy( self ) -> Void`

Release all resources held by the executor.

Shuts down if not already shut down, waits for all workers to finish, cleans up every worker process, frees the worker array, destroys the task channel, releases the future pool region, and tears down all atomic counters.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ProcessSubmitter0`<R>

Type-safe submitter for zero-argument functions to a ProcessPoolExecutor.

Wraps the executor's raw run method, converting Callable values to raw function pointers and returning typed ProcessFuture<R> instances.

### Fields

| Name | Type | Access |
|------|------|--------|
| `executor` | `ProcessPoolExecutor` | protect |

### Methods

#### `function ProcessSubmitter0( self, ProcessPoolExecutor executor ) -> Void`

Construct a new submitter bound to the given executor.

**Parameters**:

- `executor` (`ProcessPoolExecutor`)
- `The executor that will execute submitted functions.`

#### `function submit( self, <type> fn ) -> ProcessFuture<R>`

Submit a zero-argument callable for execution and return a typed future.

Converts the callable to a raw function pointer via inline assembly and delegates to the executor's run method.

**Parameters**:

- `fn` (`Callable<R, <>>`)
- `The zero-argument function to execute.`

**Returns**: — ProcessFuture<R>:
A typed future that will hold the result of the function call.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `ProcessSubmitter1`<R, A>

Type-safe submitter for one-argument functions to a ProcessPoolExecutor.

Wraps the executor's submit1 method, converting Callable values and typed arguments to raw I64 values and returning typed ProcessFuture<R> instances.

### Fields

| Name | Type | Access |
|------|------|--------|
| `executor` | `ProcessPoolExecutor` | protect |

### Methods

#### `function ProcessSubmitter1( self, ProcessPoolExecutor executor ) -> Void`

Construct a new submitter bound to the given executor.

**Parameters**:

- `executor` (`ProcessPoolExecutor`)
- `The executor that will execute submitted functions.`

#### `function submit( self, <type> fn, A arg ) -> ProcessFuture<R>`

Submit a one-argument callable for execution and return a typed future.

Converts the callable and argument to raw I64 values via inline assembly and delegates to the executor's submit1 method.

**Parameters**:

- `fn` (`Callable<R, <A>>`)
- `The one-argument function to execute.`
- `arg` (`A`)
- `The argument to pass to the function.`

**Returns**: — ProcessFuture<R>:
A typed future that will hold the result of the function call.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `ProcessSubmitter2`<R, A, B>

Type-safe submitter for two-argument functions to a ProcessPoolExecutor.

Wraps the executor's submit2 method, converting Callable values and typed arguments to raw I64 values and returning typed ProcessFuture<R> instances.

### Fields

| Name | Type | Access |
|------|------|--------|
| `executor` | `ProcessPoolExecutor` | protect |

### Methods

#### `function ProcessSubmitter2( self, ProcessPoolExecutor executor ) -> Void`

Construct a new submitter bound to the given executor.

**Parameters**:

- `executor` (`ProcessPoolExecutor`)
- `The executor that will execute submitted functions.`

#### `function submit( self, <type> fn, A arg1, B arg2 ) -> ProcessFuture<R>`

Submit a two-argument callable for execution and return a typed future.

Converts the callable and both arguments to raw I64 values via inline assembly and delegates to the executor's submit2 method.

**Parameters**:

- `fn` (`Callable<R, <A, B>>`)
- `The two-argument function to execute.`
- `arg1` (`A`)
- `The first argument to pass to the function.`
- `arg2` (`B`)
- `The second argument to pass to the function.`

**Returns**: — ProcessFuture<R>:
A typed future that will hold the result of the function call.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `ProcessSubmitter3`<R, A, B, C>

Type-safe submitter for three-argument functions to a ProcessPoolExecutor.

Wraps the executor's submit3 method, converting Callable values and typed arguments to raw I64 values and returning typed ProcessFuture<R> instances.

### Fields

| Name | Type | Access |
|------|------|--------|
| `executor` | `ProcessPoolExecutor` | protect |

### Methods

#### `function ProcessSubmitter3( self, ProcessPoolExecutor executor ) -> Void`

Construct a new submitter bound to the given executor.

**Parameters**:

- `executor` (`ProcessPoolExecutor`)
- `The executor that will execute submitted functions.`

#### `function submit( self, <type> fn, A arg1, B arg2, C arg3 ) -> ProcessFuture<R>`

Submit a three-argument callable for execution and return a typed future.

Converts the callable and all three arguments to raw I64 values via inline assembly and delegates to the executor's submit3 method.

**Parameters**:

- `fn` (`Callable<R, <A, B, C>>`)
- `The three-argument function to execute.`
- `arg1` (`A`)
- `The first argument to pass to the function.`
- `arg2` (`B`)
- `The second argument to pass to the function.`
- `arg3` (`C`)
- `The third argument to pass to the function.`

**Returns**: — ProcessFuture<R>:
A typed future that will hold the result of the function call.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

