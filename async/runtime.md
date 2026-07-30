# uranite.async.runtime

## Table of Contents

- [Imports](#imports)
- [const `scheduler`](#const-scheduler)
- [function `taskTrampoline`](#function-tasktrampoline)
- [function `runtimeInit`](#function-runtimeinit)
  - [`runtimeInit()`](#runtimeInit)
- [function `runtimeSpawn`](#function-runtimespawn)
  - [`runtimeSpawn()`](#runtimeSpawn)
- [function `runtimeAwait`](#function-runtimeawait)
  - [`runtimeAwait()`](#runtimeAwait)
- [function `runtimeYield`](#function-runtimeyield)
  - [`runtimeYield()`](#runtimeYield)
- [function `runtimeRun`](#function-runtimerun)
  - [`runtimeRun()`](#runtimeRun)
- [function `runtimeComplete`](#function-runtimecomplete)
  - [`runtimeComplete()`](#runtimeComplete)
- [function `runtimeError`](#function-runtimeerror)
  - [`runtimeError()`](#runtimeError)
- [function `runtimeHasPending`](#function-runtimehaspending)
  - [`runtimeHasPending()`](#runtimeHasPending)
- [function `runtimeAwaitFd`](#function-runtimeawaitfd)
  - [`runtimeAwaitFd()`](#runtimeAwaitFd)
- [function `runtimeSleep`](#function-runtimesleep)
  - [`runtimeSleep()`](#runtimeSleep)
- [function `runtimeTaskHasError`](#function-runtimetaskhaserror)
  - [`runtimeTaskHasError()`](#runtimeTaskHasError)
- [function `runtimeTaskGetError`](#function-runtimetaskgeterror)
  - [`runtimeTaskGetError()`](#runtimeTaskGetError)
- [function `runtimeTaskState`](#function-runtimetaskstate)
  - [`runtimeTaskState()`](#runtimeTaskState)
- [function `runtimeTaskResult`](#function-runtimetaskresult)
  - [`runtimeTaskResult()`](#runtimeTaskResult)

## Imports

- `uranite.async.scheduler`
  - `NativeScheduler`
- `uranite.async.task`
  - `FIELD_ENTRY_FN`
  - `FIELD_HAS_ERROR`
  - `FIELD_RESULT`
  - `FIELD_STATE`
  - `STATE_COMPLETED`
  - `STATE_FAILED`
- `uranite.io.syscall`
  - `readI64At`

## const `scheduler`

Global scheduler instance shared by all async operations in the program. 

## function `taskTrampoline`

### Methods

#### `function taskTrampoline(  ) -> Void`

## function `runtimeInit`

Initialize the global async runtime. Must be called once before spawning any async tasks. Sets up the epoll instance and registers the task trampoline function with the scheduler.

### Methods

#### `function runtimeInit(  ) -> Void`

Initialize the global async runtime. Must be called once before spawning any async tasks. Sets up the epoll instance and registers the task trampoline function with the scheduler.

## function `runtimeSpawn`

Spawn a new async task that will execute the function at the given address.

**Parameters**:

- `funcPtr` (`I64`)
- `Address of the function to execute as an async task.`
- `userData` (`I64`)
- `User-defined data passed to the task` (`currently unused by the scheduler`)

**Returns**: `I64` — The unique task identifier assigned to the spawned task.

### Methods

#### `function runtimeSpawn( I64 funcPtr, I64 userData ) -> I64`

Spawn a new async task that will execute the function at the given address.

**Parameters**:

- `funcPtr` (`I64`)
- `Address of the function to execute as an async task.`
- `userData` (`I64`)
- `User-defined data passed to the task` (`currently unused by the scheduler`)

**Returns**: `I64` — The unique task identifier assigned to the spawned task.

## function `runtimeAwait`

Block the current task until the specified task completes, then return its result. If the awaited task failed, raises an AsyncRuntimeError.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task to wait for.`

**Returns**: `I64` — The result value produced by the completed task.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the awaited task terminated with an error.

### Methods

#### `function runtimeAwait( I64 taskId ) -> I64`

Block the current task until the specified task completes, then return its result. If the awaited task failed, raises an AsyncRuntimeError.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task to wait for.`

**Returns**: `I64` — The result value produced by the completed task.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the awaited task terminated with an error.

## function `runtimeYield`

Voluntarily suspend the current task, allowing other tasks to run. The current task transitions to the suspended state and will be resumed in a later scheduling round.

### Methods

#### `function runtimeYield(  ) -> Void`

Voluntarily suspend the current task, allowing other tasks to run. The current task transitions to the suspended state and will be resumed in a later scheduling round.

## function `runtimeRun`

Run the scheduler event loop until all tasks have completed or failed. This is the main entry point for driving async execution to completion. After all tasks finish, performs cleanup of task resources.

### Methods

#### `function runtimeRun(  ) -> Void`

Run the scheduler event loop until all tasks have completed or failed. This is the main entry point for driving async execution to completion. After all tasks finish, performs cleanup of task resources.

## function `runtimeComplete`

Mark the current task as successfully completed with the given result value. Wakes any tasks that are awaiting this task and yields back to the scheduler.

**Parameters**:

- `result` (`I64`)
- `The result value to store for this task.`

### Methods

#### `function runtimeComplete( I64 result ) -> Void`

Mark the current task as successfully completed with the given result value. Wakes any tasks that are awaiting this task and yields back to the scheduler.

**Parameters**:

- `result` (`I64`)
- `The result value to store for this task.`

## function `runtimeError`

Mark the current task as failed with an error. Wakes any tasks that are awaiting this task and yields back to the scheduler.

**Parameters**:

- `message` (`String`)
- `Description of the error that caused the task to fail.`

### Methods

#### `function runtimeError( String message ) -> Void`

Mark the current task as failed with an error. Wakes any tasks that are awaiting this task and yields back to the scheduler.

**Parameters**:

- `message` (`String`)
- `Description of the error that caused the task to fail.`

## function `runtimeHasPending`

Check whether any tasks are still pending execution in the scheduler.

**Returns**: `Boolean` — True if at least one task is not yet completed or failed.

### Methods

#### `function runtimeHasPending(  ) -> Boolean`

Check whether any tasks are still pending execution in the scheduler.

**Returns**: `Boolean` — True if at least one task is not yet completed or failed.

## function `runtimeAwaitFd`

Suspend the current task until the specified file descriptor becomes ready for the given events. Registers the fd with epoll and yields to the scheduler.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to wait on.`
- `events` (`I64`)
- `Bitmask of epoll event flags to wait for` (`e.g., EPOLLIN`)

### Methods

#### `function runtimeAwaitFd( I64 fd, I64 events ) -> Void`

Suspend the current task until the specified file descriptor becomes ready for the given events. Registers the fd with epoll and yields to the scheduler.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to wait on.`
- `events` (`I64`)
- `Bitmask of epoll event flags to wait for` (`e.g., EPOLLIN`)

## function `runtimeSleep`

Suspend the current task for the specified duration. Uses a timerfd internally and yields to the scheduler, allowing other tasks to run while waiting.

**Parameters**:

- `milliseconds` (`I64`)
- `Duration to sleep in milliseconds.`

### Methods

#### `function runtimeSleep( I64 milliseconds ) -> Void`

Suspend the current task for the specified duration. Uses a timerfd internally and yields to the scheduler, allowing other tasks to run while waiting.

**Parameters**:

- `milliseconds` (`I64`)
- `Duration to sleep in milliseconds.`

## function `runtimeTaskHasError`

Check whether the specified task has terminated with an error.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task to check.`

**Returns**: — Boolean:
True if the task failed or was not found; False if it completed
successfully or is still running.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function runtimeTaskHasError( I64 taskId ) -> Boolean`

Check whether the specified task has terminated with an error.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task to check.`

**Returns**: — Boolean:
True if the task failed or was not found; False if it completed
successfully or is still running.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `runtimeTaskGetError`

Retrieve the error message for a failed task.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task whose error message to retrieve.`

**Returns**: `String` — A description of why the awaited task failed.

### Methods

#### `function runtimeTaskGetError( I64 taskId ) -> String`

Retrieve the error message for a failed task.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task whose error message to retrieve.`

**Returns**: `String` — A description of why the awaited task failed.

## function `runtimeTaskState`

Query the current state of a task by its identifier.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task to query.`

**Returns**: — I64:
The task's current state constant (STATE_CREATED, STATE_RUNNING,
STATE_SUSPENDED, STATE_IO_WAITING, STATE_COMPLETED, or STATE_FAILED).
Returns STATE_FAILED if the task is not found.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function runtimeTaskState( I64 taskId ) -> I64`

Query the current state of a task by its identifier.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task to query.`

**Returns**: — I64:
The task's current state constant (STATE_CREATED, STATE_RUNNING,
STATE_SUSPENDED, STATE_IO_WAITING, STATE_COMPLETED, or STATE_FAILED).
Returns STATE_FAILED if the task is not found.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `runtimeTaskResult`

Retrieve the result value of a completed task. Does not block; the task must already be finished.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task whose result to retrieve.`

**Returns**: — I64:
The result value stored by the task, or 0 if the task was not found.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function runtimeTaskResult( I64 taskId ) -> I64`

Retrieve the result value of a completed task. Does not block; the task must already be finished.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task whose result to retrieve.`

**Returns**: — I64:
The result value stored by the task, or 0 if the task was not found.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

