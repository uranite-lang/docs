# uranite.os.scheduler.scheduler

## Table of Contents

- [Imports](#imports)
- [class `Scheduler`](#class-scheduler)
  - [`Scheduler()`](#Scheduler)
  - [`setIdleTask()`](#setIdleTask)
  - [`enqueue()`](#enqueue)
  - [`dequeue()`](#dequeue)
  - [`schedule()`](#schedule)
  - [`yields()`](#yields)
  - [`tick()`](#tick)
  - [`blockCurrent()`](#blockCurrent)
  - [`unblock()`](#unblock)
  - [`allocateTaskId()`](#allocateTaskId)
  - [`getCurrentTask()`](#getCurrentTask)
  - [`getTaskCount()`](#getTaskCount)
  - [`isIdle()`](#isIdle)

## Imports

- `uranite.os.arch.cpu`
  - `disableInterrupts`
  - `enableInterrupts`
  - `halt`
- `uranite.os.task.task`
  - `Task`
  - `TaskState`

## class `Scheduler`

Round-robin preemptive task scheduler that distributes CPU time equally among all ready tasks using fixed-length time slices. The run queue is implemented as an intrusive singly-linked list using the Task.next pointer, providing O(1) enqueue and dequeue operations. Tasks are scheduled in FIFO order. When no runnable tasks are available, the scheduler falls back to an idle task that halts the CPU until the next interrupt. Context switches are triggered either by voluntary yield or by the timer interrupt handler when a task's time slice expires.

### Fields

| Name | Type | Access |
|------|------|--------|
| `runQueueHead` | `?Task` | protect |
| `runQueueTail` | `?Task` | protect |
| `currentTask` | `?Task` | protect |
| `idleTask` | `?Task` | protect |
| `taskCount` | `I64` | protect |
| `nextTaskId` | `I64` | protect |

### Methods

#### `function Scheduler( self ) -> Void`

Constructs a new scheduler with empty run queue, no current task, no idle task, and a task ID counter starting at 1.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setIdleTask( self, Task idle ) -> Void`

Sets the idle task that runs when the run queue is empty. The idle task's entry point should contain a loop that repeatedly halts the CPU (while True: halt()) to minimize power consumption while waiting for interrupts. The task is transitioned to the Ready state upon registration.

#### `function enqueue( self, ?Task task ) -> Void`

Adds a task to the end of the run queue, making it eligible for scheduling. The task is transitioned to the Ready state and its next pointer is cleared. If the queue is empty, the task becomes both the head and tail. If the task is None, no action is taken.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function dequeue( self ) -> ?Task`

Removes and returns the task at the front of the run queue. If the queue becomes empty after removal, both head and tail pointers are set to None. Returns None if the queue is already empty.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function schedule( self ) -> Void`

Selects the next task to run and performs a context switch. Interrupts are disabled during scheduling to prevent reentrancy from timer interrupts. If the current task is still runnable, it is placed back at the end of the run queue. The next task is dequeued from the front of the run queue, or the idle task is selected if the queue is empty. The selected task is marked as running and set as the current task.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function yields( self ) -> Void`

Voluntarily relinquishes the CPU by invoking the scheduler to select the next task. The current task is placed back in the run queue and a context switch is performed.

#### `function tick( self ) -> Void`

Timer interrupt handler called on each timer tick. Increments the current task's time usage counter and triggers preemptive scheduling if the task's time slice has expired.

#### `function blockCurrent( self ) -> Void`

Blocks the currently running task by transitioning it to the Blocked state, then invokes the scheduler to select the next runnable task. The caller should set the block reason on the task before calling this method so the appropriate wakeup mechanism can later unblock it.

#### `function unblock( self, ?Task task ) -> Void`

Unblocks a previously blocked task by transitioning it back to the Ready state and adding it to the end of the run queue, making it eligible for scheduling again. No action is taken if the task is None.

#### `function allocateTaskId( self ) -> I64`

Generates and returns the next unique task identifier. Task IDs are monotonically increasing integers starting from 1, ensuring each task in the system has a distinct identifier.

#### `function getCurrentTask( self ) -> ?Task`

Returns the currently running task, or None if no task is executing. 

#### `function getTaskCount( self ) -> I64`

Returns the number of tasks currently waiting in the run queue. 

#### `function isIdle( self ) -> Boolean`

Returns True if the scheduler is currently running the idle task or if no task is running at all, indicating that no user tasks are ready for execution.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

