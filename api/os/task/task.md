# uranite.os.task.task

## Table of Contents

- [Imports](#imports)
- [enum `TaskState`](#enum-taskstate)
- [enum `BlockReason`](#enum-blockreason)
- [class `Task`](#class-task)
  - [`Task()`](#Task)
  - [`setEntryPoint()`](#setEntryPoint)
  - [`makeReady()`](#makeReady)
  - [`markRunning()`](#markRunning)
  - [`block()`](#block)
  - [`unblock()`](#unblock)
  - [`terminate()`](#terminate)
  - [`isRunnable()`](#isRunnable)
  - [`isBlocked()`](#isBlocked)
  - [`isTerminated()`](#isTerminated)
  - [`isTimeSliceExpired()`](#isTimeSliceExpired)
  - [`tick()`](#tick)
  - [`setNext()`](#setNext)
  - [`clearNext()`](#clearNext)
  - [`getNext()`](#getNext)
  - [`getState()`](#getState)
  - [`setState()`](#setState)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.arch.registers`
  - `Registers`
- `uranite.os.memory.address-space`
  - `AddressSpace`
- `uranite.os.security.handle`
  - `HandleTable`

## enum `TaskState`

Enumerates the lifecycle states of a kernel task. Tasks transition between these states as they are created, scheduled, blocked on resources, and eventually terminated.

## enum `BlockReason`

Identifies the reason a task is blocked, enabling targeted wakeup by the subsystem that owns the blocking resource. Each variant corresponds to a different kernel synchronization or I/O mechanism.

## class `Task`

Kernel execution unit representing a thread or process. A task holds everything needed to pause execution, switch to another task, and later resume: saved CPU registers, stack information, virtual address space reference, handle table for kernel object access, scheduling priority, and time slice tracking. Tasks are linked together in the scheduler's run queue via an intrusive next pointer.

### Fields

| Name | Type | Access |
|------|------|--------|
| `id` | `I64` | public |
| `state` | `TaskState` | public |
| `blockReason` | `BlockReason` | public |
| `registers` | `Registers` | public |
| `stackBase` | `I64` | public |
| `stackSize` | `I64` | public |
| `addressSpace` | `?AddressSpace` | public |
| `handles` | `HandleTable` | public |
| `priority` | `I64` | public |
| `timeSlice` | `I64` | public |
| `timeUsed` | `I64` | public |
| `next` | `?Task` | public |

### Methods

#### `function Task( self, I64 id, I64 stackSize, I64 priority ) -> Void`

Constructs a new task with the given unique identifier, stack size in bytes, and scheduling priority. The task starts in the Created state with zeroed registers, no address space, a handle table supporting 256 handles, a default time slice of 10 ticks, and no next pointer in the run queue.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setEntryPoint( self, I64 entryAddr, I64 stackTop ) -> Void`

Initializes the task's entry point by setting the instruction pointer (RIP) and stack pointer (RSP) in the saved register context. The stack base is calculated as stackTop minus the configured stack size. When the scheduler first switches to this task, execution begins at entryAddr with the stack pointer at stackTop.

#### `function makeReady( self ) -> Void`

Transitions the task to the Ready state, making it eligible for scheduling by the run queue. Clears any block reason and resets the time usage counter to zero for a fresh time slice.

#### `function markRunning( self ) -> Void`

Marks the task as currently running on the CPU. This is called by the scheduler when this task is selected for execution.

#### `function block( self, BlockReason reason ) -> Void`

Blocks the task with the specified reason, removing it from the run queue until the blocking condition is resolved. The block reason enables targeted wakeup by the appropriate kernel subsystem (mutex, semaphore, I/O completion, etc.).

#### `function unblock( self ) -> Void`

Unblocks the task by transitioning it back to the Ready state and clearing the block reason, making it eligible for scheduling again.

#### `function terminate( self ) -> Void`

Permanently terminates the task by setting its state to Terminated and destroying its handle table to release all kernel object references. A terminated task cannot be resumed or rescheduled.

#### `function isRunnable( self ) -> Boolean`

Returns True if the task is in the Ready or Running state and able to execute. 

#### `function isBlocked( self ) -> Boolean`

Returns True if the task is in the Blocked state, waiting on a resource. 

#### `function isTerminated( self ) -> Boolean`

Returns True if the task has been permanently terminated. 

#### `function isTimeSliceExpired( self ) -> Boolean`

Returns True if the task has consumed its entire time slice, indicating that preemptive scheduling should occur to give other tasks a turn on the CPU.

#### `function tick( self ) -> Void`

Increments the time usage counter by one tick. Called from the timer interrupt handler on each timer tick while this task is running.

#### `function setNext( self, ?Task t ) -> Void`

Sets the next pointer for intrusive linked list chaining in the run queue. 

#### `function clearNext( self ) -> Void`

Clears the next pointer by setting it to None, unlinking this task from the run queue. 

#### `function getNext( self ) -> ?Task`

Returns the next task in the run queue linked list, or None if this is the last task. 

#### `function getState( self ) -> TaskState`

Returns the current lifecycle state of this task. 

#### `function setState( self, TaskState s ) -> Void`

Sets the lifecycle state of this task to the specified value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

