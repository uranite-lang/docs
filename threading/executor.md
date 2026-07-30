# uranite.threading.executor

## Table of Contents

- [Imports](#imports)
- [const `DEFAULT_QUEUE_CAPACITY`](#const-default-queue-capacity)
- [const `EXECUTOR_WORKER_STACK_PAGES`](#const-executor-worker-stack-pages)
- [function `executorWorkerLoop`](#function-executorworkerloop)
  - [`executorWorkerLoop()`](#executorWorkerLoop)
- [class `ThreadPoolExecutor`](#class-threadpoolexecutor)
  - [`ThreadPoolExecutor()`](#ThreadPoolExecutor)
  - [`shutdown()`](#shutdown)
  - [`isShutdown()`](#isShutdown)
  - [`getActiveCount()`](#getActiveCount)
  - [`getCompletedCount()`](#getCompletedCount)
  - [`getWorkerCount()`](#getWorkerCount)
  - [`shutdownAndAwait()`](#shutdownAndAwait)
  - [`destroy()`](#destroy)
- [class `TaskSubmitter0`](#class-tasksubmitter0)
  - [`TaskSubmitter0()`](#TaskSubmitter0)
  - [`submit()`](#submit)
- [class `TaskSubmitter1`](#class-tasksubmitter1)
  - [`TaskSubmitter1()`](#TaskSubmitter1)
  - [`submit()`](#submit)
- [class `TaskSubmitter2`](#class-tasksubmitter2)
  - [`TaskSubmitter2()`](#TaskSubmitter2)
  - [`submit()`](#submit)
- [class `TaskSubmitter3`](#class-tasksubmitter3)
  - [`TaskSubmitter3()`](#TaskSubmitter3)
  - [`submit()`](#submit)

## Imports

- `uranite.language.callable`
  - `Callable`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.task.task`
  - `BlockReason`
  - `Task`
- `uranite.threading.atomic`
  - `AtomicBoolean`
  - `AtomicI64`
- `uranite.threading.channel`
  - `ChannelInner`
- `uranite.threading.errors`
  - `ThreadError`
- `uranite.threading.future`
  - `FUTURE_ERROR`
  - `FUTURE_FINISHED`
  - `FUTURE_PENDING`
  - `FUTURE_RUNNING`
  - `Future`
  - `FutureBase`
- `uranite.threading.thread`
  - `DEFAULT_PRIORITY`
  - `DEFAULT_STACK_PAGES`
  - `Thread`
  - `ThreadRuntime`

## const `DEFAULT_QUEUE_CAPACITY`

Default capacity for the executor's internal task channel queue.

## const `EXECUTOR_WORKER_STACK_PAGES`

Number of memory pages allocated for each worker thread's stack.

## function `executorWorkerLoop`

Entry point for each worker thread in the executor pool. Continuously receives task descriptors from the shared channel, dispatches the function call via inline assembly indirect call, and stores the result or error in the associated FutureBase.

The descriptor is a Memory<I64>(6) block with arity, function address, FutureBase address, and up to three arguments. The worker loop exits when the channel is closed or a zero descriptor address is received.

**Parameters**:

- `threadId` (`I64`)
- `Unique identifier of this worker thread, unused in the loop`
- `body but required by the thread entry point calling convention.`
- `channelAddr` (`I64`)
- `Raw memory address of the ChannelInner<I64> used to receive`
- `task descriptors from the executor.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function executorWorkerLoop( I64 threadId, I64 channelAddr ) -> Void`

Entry point for each worker thread in the executor pool. Continuously receives task descriptors from the shared channel, dispatches the function call via inline assembly indirect call, and stores the result or error in the associated FutureBase.

The descriptor is a Memory<I64>(6) block with arity, function address, FutureBase address, and up to three arguments. The worker loop exits when the channel is closed or a zero descriptor address is received.

**Parameters**:

- `threadId` (`I64`)
- `Unique identifier of this worker thread, unused in the loop`
- `body but required by the thread entry point calling convention.`
- `channelAddr` (`I64`)
- `Raw memory address of the ChannelInner<I64> used to receive`
- `task descriptors from the executor.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `ThreadPoolExecutor`

High-level thread pool executor that manages a fixed set of worker threads and dispatches callable tasks to them via a shared channel. Each submitted task returns a FutureBase that can be polled or awaited for the result. Workers use inline assembly indirect calls to invoke the target function with up to three I64 arguments.

Built entirely on bare-metal OS modules with no C/C++ runtime dependency. Use TaskSubmitter0 through TaskSubmitter3 for type-safe generic submission.

### Fields

| Name | Type | Access |
|------|------|--------|
| `workers` | `Memory<Thread>` | protect |
| `workerCount` | `I64` | protect |
| `taskChannel` | `ChannelInner<I64>` | protect |
| `runtime` | `ThreadRuntime` | protect |
| `activeCount` | `AtomicI64` | protect |
| `completedCount` | `AtomicI64` | protect |
| `shutdownFlag` | `AtomicBoolean` | protect |

### Methods

#### `function ThreadPoolExecutor( self, I64 numWorkers, ?ThreadRuntime runtime ) -> Void`

Create a new thread pool executor with the specified number of worker threads. If no ThreadRuntime is provided, a default runtime is created with a new Scheduler, PhysicalMemoryManager (1 MB), SystemClock, and capacity for 256 tasks.

**Parameters**:

- `numWorkers` (`I64`)
- `Number of worker threads to spawn. Must be at least 1.`
- `runtime` (`?ThreadRuntime`)
- `Optional pre-configured thread runtime. When None, a default`
- `runtime is created internally.`

**Raises**:

- `ThreadError` → `Error` — If numWorkers is less than or equal to zero.

**Complexity**:
- Time: `O(w) where w is numWorkers`
- Space: `O(w)`

#### `function run( self, I64 fnAddr ) -> FutureBase`

Submit a zero-argument function for execution on a worker thread. The function is identified by its raw address and called with no arguments. Returns immediately with a FutureBase that tracks the asynchronous result.

**Parameters**:

- `fnAddr` (`I64`)
- `Raw address of the function to execute. The function must`
- `accept zero arguments and return an I64.`

**Returns**: `FutureBase` — A future representing the pending result of the function call.

**Raises**:

- `ThreadError` → `Error` — If the executor has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function submit1( self, I64 fnAddr, I64 arg ) -> FutureBase`

Submit a one-argument function for execution on a worker thread.

**Parameters**:

- `fnAddr` (`I64`)
- `Raw address of the function to execute. The function must`
- `accept one I64 argument and return an I64.`
- `arg` (`I64`)
- `The single argument to pass to the function via the rdi`
- `register.`

**Returns**: `FutureBase` — A future representing the pending result of the function call.

**Raises**:

- `ThreadError` → `Error` — If the executor has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function submit2( self, I64 fnAddr, I64 arg1, I64 arg2 ) -> FutureBase`

Submit a two-argument function for execution on a worker thread.

**Parameters**:

- `fnAddr` (`I64`)
- `Raw address of the function to execute. The function must`
- `accept two I64 arguments and return an I64.`
- `arg1` (`I64`)
- `First argument, passed via the rdi register.`
- `arg2` (`I64`)
- `Second argument, passed via the rsi register.`

**Returns**: `FutureBase` — A future representing the pending result of the function call.

**Raises**:

- `ThreadError` → `Error` — If the executor has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function submit3( self, I64 fnAddr, I64 arg1, I64 arg2, I64 arg3 ) -> FutureBase`

Submit a three-argument function for execution on a worker thread.

**Parameters**:

- `fnAddr` (`I64`)
- `Raw address of the function to execute. The function must`
- `accept three I64 arguments and return an I64.`
- `arg1` (`I64`)
- `First argument, passed via the rdi register.`
- `arg2` (`I64`)
- `Second argument, passed via the rsi register.`
- `arg3` (`I64`)
- `Third argument, passed via the rdx register.`

**Returns**: `FutureBase` — A future representing the pending result of the function call.

**Raises**:

- `ThreadError` → `Error` — If the executor has been shut down.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function shutdown( self, Boolean wait ) -> Void`

Initiate executor shutdown. Sets the shutdown flag and closes the task channel so workers stop receiving new descriptors. Optionally waits for all worker threads to finish their current tasks.

**Parameters**:

- `wait` (`Boolean`)
- `When True, blocks until every worker thread has finished`
- `execution. When False, returns immediately after signaling`
- `shutdown.`

**Complexity**:
- Time: `O(w) where w is workerCount when wait=True, O(1) otherwise`
- Space: `O(1)`

#### `function isShutdown( self ) -> Boolean`

Check whether the executor has been shut down.

**Returns**: `Boolean` — True if shutdown has been initiated, False otherwise.

#### `function getActiveCount( self ) -> I64`

Return the number of tasks currently in flight (submitted but not yet completed).

**Returns**: `I64` — Count of active tasks.

#### `function getCompletedCount( self ) -> I64`

Return the total number of tasks that have completed execution.

**Returns**: `I64` — Cumulative count of completed tasks.

#### `function getWorkerCount( self ) -> I64`

Return the number of worker threads in the pool.

**Returns**: `I64` — The fixed worker count configured at construction.

#### `function shutdownAndAwait( self ) -> Void`

Graceful shutdown: stop accepting new tasks, wait for all running tasks to complete, then release all resources.

Equivalent to calling shutdown(True) followed by destroy().

**Complexity**:
- Time: `O(w) where w is workerCount`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release all resources held by the executor. If not already shut down, performs a blocking shutdown first. Frees worker storage, task channel, and all atomic counters.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `TaskSubmitter0`<R>

Type-safe wrapper for submitting zero-argument callables to a ThreadPoolExecutor. Converts the Callable into a raw function address and returns a typed Future<R> instead of an untyped FutureBase.

### Fields

| Name | Type | Access |
|------|------|--------|
| `executor` | `ThreadPoolExecutor` | protect |

### Methods

#### `function TaskSubmitter0( self, ThreadPoolExecutor executor ) -> Void`

Create a new submitter bound to the given executor.

**Parameters**:

- `executor` (`ThreadPoolExecutor`)
- `The executor pool to submit tasks to.`

#### `function submit( self, <type> fn ) -> Future<R>`

Submit a zero-argument callable for asynchronous execution.

**Parameters**:

- `fn` (`Callable<R, <>>`)
- `The callable to execute. Must accept no arguments and`
- `return a value of type R.`

**Returns**: — Future<R>:
A typed future that will contain the result of the callable
once execution completes.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `TaskSubmitter1`<R, A>

Type-safe wrapper for submitting one-argument callables to a ThreadPoolExecutor. Converts the Callable and its argument into raw I64 values and returns a typed Future<R>.

### Fields

| Name | Type | Access |
|------|------|--------|
| `executor` | `ThreadPoolExecutor` | protect |

### Methods

#### `function TaskSubmitter1( self, ThreadPoolExecutor executor ) -> Void`

Create a new submitter bound to the given executor.

**Parameters**:

- `executor` (`ThreadPoolExecutor`)
- `The executor pool to submit tasks to.`

#### `function submit( self, <type> fn, A arg ) -> Future<R>`

Submit a one-argument callable for asynchronous execution.

**Parameters**:

- `fn` (`Callable<R, <A>>`)
- `The callable to execute. Must accept one argument of type A`
- `and return a value of type R.`
- `arg` (`A`)
- `The argument to pass to the callable.`

**Returns**: — Future<R>:
A typed future that will contain the result of the callable
once execution completes.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `TaskSubmitter2`<R, A, B>

Type-safe wrapper for submitting two-argument callables to a ThreadPoolExecutor. Converts the Callable and both arguments into raw I64 values and returns a typed Future<R>.

### Fields

| Name | Type | Access |
|------|------|--------|
| `executor` | `ThreadPoolExecutor` | protect |

### Methods

#### `function TaskSubmitter2( self, ThreadPoolExecutor executor ) -> Void`

Create a new submitter bound to the given executor.

**Parameters**:

- `executor` (`ThreadPoolExecutor`)
- `The executor pool to submit tasks to.`

#### `function submit( self, <type> fn, A arg1, B arg2 ) -> Future<R>`

Submit a two-argument callable for asynchronous execution.

**Parameters**:

- `fn` (`Callable<R, <A, B>>`)
- `The callable to execute. Must accept two arguments of types`
- `A and B, and return a value of type R.`
- `arg1` (`A`)
- `The first argument to pass to the callable.`
- `arg2` (`B`)
- `The second argument to pass to the callable.`

**Returns**: — Future<R>:
A typed future that will contain the result of the callable
once execution completes.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `TaskSubmitter3`<R, A, B, C>

Type-safe wrapper for submitting three-argument callables to a ThreadPoolExecutor. Converts the Callable and all three arguments into raw I64 values and returns a typed Future<R>.

### Fields

| Name | Type | Access |
|------|------|--------|
| `executor` | `ThreadPoolExecutor` | protect |

### Methods

#### `function TaskSubmitter3( self, ThreadPoolExecutor executor ) -> Void`

Create a new submitter bound to the given executor.

**Parameters**:

- `executor` (`ThreadPoolExecutor`)
- `The executor pool to submit tasks to.`

#### `function submit( self, <type> fn, A arg1, B arg2, C arg3 ) -> Future<R>`

Submit a three-argument callable for asynchronous execution.

**Parameters**:

- `fn` (`Callable<R, <A, B, C>>`)
- `The callable to execute. Must accept three arguments of`
- `types A, B, and C, and return a value of type R.`
- `arg1` (`A`)
- `The first argument to pass to the callable.`
- `arg2` (`B`)
- `The second argument to pass to the callable.`
- `arg3` (`C`)
- `The third argument to pass to the callable.`

**Returns**: — Future<R>:
A typed future that will contain the result of the callable
once execution completes.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

