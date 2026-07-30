# uranite.async.scheduler

## Table of Contents

- [Imports](#imports)
- [const `MAX_TASKS`](#const-max-tasks)
- [const `MAX_EPOLL_EVENTS`](#const-max-epoll-events)
- [const `SYS_MUNMAP`](#const-sys-munmap)
- [class `NativeScheduler`](#class-nativescheduler)
  - [`NativeScheduler()`](#NativeScheduler)
  - [`init()`](#init)
  - [`spawn()`](#spawn)
  - [`spawn()`](#spawn)
  - [`findTaskIndex()`](#findTaskIndex)
  - [`awaitTask()`](#awaitTask)
  - [`taskYield()`](#taskYield)
  - [`awaitFd()`](#awaitFd)
  - [`sleep()`](#sleep)
  - [`taskComplete()`](#taskComplete)
  - [`taskError()`](#taskError)
  - [`wakeWaiters()`](#wakeWaiters)
  - [`hasPending()`](#hasPending)
  - [`runOnce()`](#runOnce)
  - [`pollIO()`](#pollIO)
  - [`run()`](#run)
  - [`runLoop()`](#runLoop)
  - [`cleanup()`](#cleanup)
  - [`destroy()`](#destroy)

## Imports

- `uranite.async.context`
  - `swapContext`
- `uranite.async.epoll`
  - `EPOLLIN`
  - `allocateEventBuffer`
  - `epollAdd`
  - `epollCreate`
  - `epollDel`
  - `epollEventGetFd`
  - `epollWait`
- `uranite.async.errors`
  - `AsyncRuntimeError`
- `uranite.async.task`
  - `FIELD_AWAITING_ID`
  - `FIELD_AWAIT_EVENTS`
  - `FIELD_AWAIT_FD`
  - `FIELD_CONTEXT_RSP`
  - `FIELD_ENTRY_FN`
  - `FIELD_HAS_ERROR`
  - `FIELD_ID`
  - `FIELD_RESULT`
  - `FIELD_STACK_BASE`
  - `FIELD_STACK_SIZE`
  - `FIELD_STATE`
  - `STATE_COMPLETED`
  - `STATE_CREATED`
  - `STATE_FAILED`
  - `STATE_IO_WAITING`
  - `STATE_RUNNING`
  - `STATE_SUSPENDED`
  - `createTask`
  - `taskAllocateStack`
  - `taskInitContext`
  - `taskRspAddr`
- `uranite.async.timer`
  - `timerfdClose`
  - `timerfdCreate`
  - `timerfdSettime`
- `uranite.coroutine.scheduler`
  - `Scheduler`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readI64At`
  - `sysClose`
  - `writeI64At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall2`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `MAX_TASKS`

Maximum number of concurrent tasks the scheduler can manage. 

## const `MAX_EPOLL_EVENTS`

Maximum number of epoll events processed per poll cycle. 

## const `SYS_MUNMAP`

Syscall number for munmap on x86-64 Linux. 

## class `NativeScheduler`

**Implements**: `Scheduler`

Cooperative async task scheduler built on x86-64 context switching and Linux epoll. Manages a fixed-size pool of task slots, each with its own stack and execution context. Tasks run cooperatively by yielding back to the scheduler, which multiplexes I/O readiness via epoll.

### Fields

| Name | Type | Access |
|------|------|--------|
| `taskSlots` | `Memory<I64>` | public |
| `taskCount` | `I64` | public |
| `nextTaskId` | `I64` | public |
| `currentTaskIdx` | `I64` | public |
| `epollFd` | `I64` | public |
| `mainRspBuf` | `Memory<I64>` | public |
| `epollEventBuf` | `Memory<I64>` | public |
| `trampolineAddr` | `I64` | public |
| `initialized` | `Boolean` | public |

### Methods

#### `function NativeScheduler( self ) -> Void`

Construct a new scheduler with empty task slots and uninitialized epoll. Call init() before spawning any tasks.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function init( self, I64 trampoline ) -> Void`

Initialize the scheduler by creating the epoll instance and registering the task trampoline function. Must be called once before spawning tasks.

**Parameters**:

- `trampoline` (`I64`)
- `Address of the trampoline function that bootstraps task execution.`

#### `function spawn( self, I64 funcPtr ) -> I64`

Create a new task from a function pointer with no user data. Satisfies the Scheduler interface contract.

**Parameters**:

- `funcPtr` (`I64`)
- `Address of the function the task will execute.`

**Returns**: `I64` — The unique identifier assigned to the newly spawned task.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the maximum task count has been reached or no free slot exists.

#### `function spawn( self, I64 funcPtr, I64 userData ) -> I64`

Create a new task and allocate its stack and execution context. The task is placed in the first available slot and assigned a unique identifier.

**Parameters**:

- `funcPtr` (`I64`)
- `Address of the function the task will execute.`
- `userData` (`I64`)
- `User-defined data associated with the task.`

**Returns**: `I64` — The unique identifier assigned to the newly spawned task.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the maximum task count has been reached or no free slot exists.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function findTaskIndex( self, I64 taskId ) -> I64`

Search for a task by its identifier and return its slot index.

**Parameters**:

- `taskId` (`I64`)
- `The unique identifier of the task to locate.`

**Returns**: — I64:
The slot index containing the task, or -1 if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function awaitTask( self, I64 taskId ) -> I64`

Block the current task until the target task completes or fails. If called from within a task, suspends it and records the dependency. If called from the main context, runs scheduler rounds until the target finishes.

**Parameters**:

- `taskId` (`I64`)
- `Identifier of the task to wait for.`

**Returns**: `I64` — The result value produced by the completed task.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the task is not found or the awaited task terminated with an error.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function taskYield( self ) -> Void`

Voluntarily suspend the currently running task and return control to the scheduler. The task will be resumed in a later scheduling round. Has no effect if called from outside a task context.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function awaitFd( self, I64 fd, I64 events ) -> Void`

Suspend the current task until the specified file descriptor becomes ready for the given events. Registers the fd with epoll and transitions the task to the I/O waiting state.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to wait on.`
- `events` (`I64`)
- `Bitmask of epoll event flags to monitor.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function sleep( self, I64 milliseconds ) -> Void`

Suspend the current task for the specified duration using a timerfd. Creates a one-shot timer, waits for it via epoll, then closes the timer file descriptor.

**Parameters**:

- `milliseconds` (`I64`)
- `Duration to sleep in milliseconds.`

#### `function taskComplete( self, I64 result ) -> Void`

Mark the current task as successfully completed with the given result. Stores the result, transitions the task to completed state, wakes any tasks awaiting this one, and yields back to the scheduler.

**Parameters**:

- `result` (`I64`)
- `The result value to store for this task.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function taskError( self, String message ) -> Void`

Mark the current task as failed. Sets the error flag, transitions the task to the failed state, wakes any tasks awaiting this one, and yields back to the scheduler.

**Parameters**:

- `message` (`String`)
- `Description of the error` (`stored for diagnostic purposes`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function wakeWaiters( self, I64 finishedTaskId ) -> Void`

Resume all tasks that are blocked awaiting the specified task. Clears each waiter's awaiting ID and transitions it back to the suspended state so it can be scheduled again.

**Parameters**:

- `finishedTaskId` (`I64`)
- `The identifier of the task that just completed or failed.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function hasPending( self ) -> Boolean`

Check whether any tasks are still pending, meaning they have not yet reached the completed or failed state.

**Returns**: — Boolean:
True if at least one task is still created, running, suspended,
or waiting on I/O.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function runOnce( self ) -> Void`

Execute a single scheduling round. Scans for the first runnable task (created or suspended), switches to it, and returns after it yields. If no runnable task is found, polls for I/O readiness to unblock waiting tasks.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function pollIO( self ) -> Void`

Poll for I/O readiness using epoll and wake any tasks whose awaited file descriptors are ready. Uses a 100ms timeout to avoid busy-waiting. Each ready fd is matched against I/O-waiting tasks, which are transitioned back to the suspended state and removed from epoll.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function run( self ) -> Void`

Run the scheduler event loop until all tasks have completed or failed, then perform resource cleanup.

#### `function runLoop( self ) -> Void`

Execute scheduling rounds in a loop until no pending tasks remain. Each iteration calls runOnce() which either runs a task or polls I/O.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function cleanup( self ) -> Void`

Release all task resources. Unmaps each task's stack via munmap and clears all task slots. Resets the task count to zero.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Fully tear down the scheduler. Cleans up all tasks, closes the epoll file descriptor, and releases all internal memory.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

