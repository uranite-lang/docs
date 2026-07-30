# uranite.threading.thread

## Table of Contents

- [Imports](#imports)
- [const `DEFAULT_STACK_PAGES`](#const-default-stack-pages)
- [const `PAGE_SIZE`](#const-page-size)
- [const `DEFAULT_PRIORITY`](#const-default-priority)
- [const `STATE_NEW`](#const-state-new)
- [const `STATE_RUNNING`](#const-state-running)
- [const `STATE_FINISHED`](#const-state-finished)
- [const `STATE_DETACHED`](#const-state-detached)
- [class `ThreadRuntime`](#class-threadruntime)
  - [`ThreadRuntime()`](#ThreadRuntime)
  - [`allocateStack()`](#allocateStack)
  - [`freeStack()`](#freeStack)
  - [`allocateThreadId()`](#allocateThreadId)
  - [`registerTask()`](#registerTask)
  - [`lookupTaskPointer()`](#lookupTaskPointer)
  - [`unregisterTask()`](#unregisterTask)
  - [`getScheduler()`](#getScheduler)
  - [`getPmm()`](#getPmm)
  - [`getClock()`](#getClock)
  - [`destroy()`](#destroy)
- [class `ThreadConfig`](#class-threadconfig)
  - [`ThreadConfig()`](#ThreadConfig)
  - [`setStackSize()`](#setStackSize)
  - [`setPriority()`](#setPriority)
  - [`setName()`](#setName)
- [class `Thread`](#class-thread)
  - [`Thread()`](#Thread)
  - [`start()`](#start)
  - [`getThreadId()`](#getThreadId)
  - [`getTask()`](#getTask)
  - [`getCompletionEvent()`](#getCompletionEvent)
  - [`setResult()`](#setResult)
  - [`getResult()`](#getResult)
  - [`markFinished()`](#markFinished)
  - [`markPanicked()`](#markPanicked)
  - [`hasPanicked()`](#hasPanicked)
  - [`isFinished()`](#isFinished)
  - [`isDetached()`](#isDetached)
  - [`detach()`](#detach)
  - [`cleanup()`](#cleanup)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.memory.pmm`
  - `PhysicalMemoryManager`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.sync.spinlock`
  - `Spinlock`
- `uranite.os.task.task`
  - `BlockReason`
  - `Task`
  - `TaskState`
- `uranite.os.time.clock`
  - `SystemClock`
- `uranite.threading.atomic`
  - `AtomicBoolean`
  - `AtomicI64`
- `uranite.threading.errors`
  - `ThreadError`

## const `DEFAULT_STACK_PAGES`

Default number of memory pages allocated for a thread's stack.

## const `PAGE_SIZE`

Size of a single memory page in bytes.

## const `DEFAULT_PRIORITY`

Default scheduling priority for newly created threads.

## const `STATE_NEW`

Thread state indicating it has been created but not yet started.

## const `STATE_RUNNING`

Thread state indicating it is currently executing.

## const `STATE_FINISHED`

Thread state indicating it has completed execution.

## const `STATE_DETACHED`

Thread state indicating it has been detached from its owner.

## class `ThreadRuntime`

Central runtime providing access to kernel subsystems needed for threading: the task scheduler, physical memory manager, system clock, and a task pointer registry. All threads in a program share a single ThreadRuntime instance which manages thread ID allocation, stack allocation/deallocation, and task registration.

Built entirely on bare-metal OS modules with no C/C++ runtime dependency.

### Fields

| Name | Type | Access |
|------|------|--------|
| `scheduler` | `Scheduler` | protect |
| `pmm` | `PhysicalMemoryManager` | protect |
| `clock` | `SystemClock` | protect |
| `guard` | `Spinlock` | protect |
| `nextThreadId` | `AtomicI64` | protect |
| `initialized` | `AtomicBoolean` | protect |
| `taskPointers` | `Memory<I64>` | protect |
| `maxTasks` | `I64` | protect |

### Methods

#### `function ThreadRuntime( self, Scheduler scheduler, PhysicalMemoryManager pmm, SystemClock clock, I64 maxTasks ) -> Void`

Create a new thread runtime with the given kernel subsystems.

**Parameters**:

- `scheduler` (`Scheduler`)
- `The kernel task scheduler for enqueuing and managing threads.`
- `pmm` (`PhysicalMemoryManager`)
- `The physical memory manager for stack allocation.`
- `clock` (`SystemClock`)
- `The system clock for timing operations.`
- `maxTasks` (`I64`)
- `Maximum number of concurrent tasks the runtime can track.`
- `Task IDs must be less than this value.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function allocateStack( self, I64 stackPages ) -> I64`

Allocate a contiguous region of physical memory for a thread stack. Allocates one extra page beyond the requested size for a guard page.

**Parameters**:

- `stackPages` (`I64`)
- `Number of pages for the usable stack area.`

**Returns**: `I64` — Base address of the allocated stack region.

#### `function freeStack( self, I64 stackBase, I64 stackPages ) -> Void`

Free a previously allocated thread stack region.

**Parameters**:

- `stackBase` (`I64`)
- `Base address of the stack region to free.`
- `stackPages` (`I64`)
- `Number of usable stack pages (the guard page is freed`
- `automatically).`

#### `function allocateThreadId( self ) -> I64`

Atomically allocate a unique thread identifier.

**Returns**: `I64` — A unique, monotonically increasing thread ID.

#### `function registerTask( self, I64 taskId, I64 taskPointer ) -> Void`

Register a task pointer in the runtime's task registry. Protected by a spinlock for thread safety.

**Parameters**:

- `taskId` (`I64`)
- `The task identifier to register. Must be greater than 0`
- `and less than maxTasks.`
- `taskPointer` (`I64`)
- `Raw pointer to the task structure.`

#### `function lookupTaskPointer( self, I64 taskId ) -> I64`

Look up a task pointer by its identifier.

**Parameters**:

- `taskId` (`I64`)
- `The task identifier to look up.`

**Returns**: `I64` — The raw task pointer, or 0 if the task ID is out of range or not registered.

#### `function unregisterTask( self, I64 taskId ) -> Void`

Remove a task from the runtime's task registry. Protected by a spinlock for thread safety.

**Parameters**:

- `taskId` (`I64`)
- `The task identifier to unregister.`

#### `function getScheduler( self ) -> Scheduler`

Return the kernel task scheduler.

**Returns**: `Scheduler` — The scheduler instance managed by this runtime.

#### `function getPmm( self ) -> PhysicalMemoryManager`

Return the physical memory manager.

**Returns**: `PhysicalMemoryManager` — The PMM instance managed by this runtime.

#### `function getClock( self ) -> SystemClock`

Return the system clock.

**Returns**: `SystemClock` — The clock instance managed by this runtime.

#### `function destroy( self ) -> Void`

Release all resources held by the runtime, including the task pointer registry and atomic counters.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `ThreadConfig`

Builder for configuring thread parameters before spawning. Provides a fluent interface for setting stack size, scheduling priority, and thread name. Each setter returns the config instance for method chaining.

### Fields

| Name | Type | Access |
|------|------|--------|
| `stackPages` | `I64` | public |
| `priority` | `I64` | public |
| `name` | `String` | public |

### Methods

#### `function ThreadConfig( self ) -> Void`

Create a new thread configuration with default values: 8 stack pages, priority 5, and an empty name.

#### `function setStackSize( self, I64 pages ) -> ThreadConfig`

Set the number of stack pages for the thread.

**Parameters**:

- `pages` (`I64`)
- `Number of memory pages to allocate for the stack.`

**Returns**: `ThreadConfig` — This configuration instance for method chaining.

#### `function setPriority( self, I64 prio ) -> ThreadConfig`

Set the scheduling priority for the thread.

**Parameters**:

- `prio` (`I64`)
- `The priority level. Higher values indicate higher priority.`

**Returns**: `ThreadConfig` — This configuration instance for method chaining.

#### `function setName( self, String threadName ) -> ThreadConfig`

Set a human-readable name for the thread.

**Parameters**:

- `threadName` (`String`)
- `The thread name, used for debugging and identification.`

**Returns**: — ThreadConfig:
This configuration instance for method chaining.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Thread`

Individual execution context with its own stack, task, and lifecycle state. Wraps an OS-level Task with thread-specific state management including completion signaling, result storage, and panic tracking.

A thread progresses through states: New -> Running -> Finished (or Detached). The completion event is signaled when the thread finishes, allowing JoinHandle or other waiters to be notified.

### Fields

| Name | Type | Access |
|------|------|--------|
| `task` | `Task` | protect |
| `threadId` | `I64` | protect |
| `stackBasePhys` | `I64` | protect |
| `stackPages` | `I64` | protect |
| `runtime` | `ThreadRuntime` | protect |
| `completionEvent` | `Event` | protect |
| `state` | `AtomicI64` | protect |
| `resultSlot` | `Memory<I64>` | protect |
| `panicked` | `AtomicBoolean` | protect |

### Methods

#### `function Thread( self, ThreadRuntime runtime, I64 threadId, I64 stackPages, I64 priority ) -> Void`

Create a new thread with the given configuration. Allocates a stack from the runtime's physical memory manager, creates an OS task with the specified priority, and registers the task in the runtime.

The thread is created in the New state and must be started via start() to begin execution.

**Parameters**:

- `runtime` (`ThreadRuntime`)
- `The shared thread runtime for stack allocation and scheduling.`
- `threadId` (`I64`)
- `Unique identifier for this thread.`
- `stackPages` (`I64`)
- `Number of memory pages to allocate for the thread's stack.`
- `priority` (`I64`)
- `Scheduling priority for the thread's underlying task.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function start( self, I64 entryAddr ) -> Void`

Start the thread's execution at the given function address. Sets the entry point, passes the thread ID in rdi, transitions the state to Running, and enqueues the task in the scheduler.

**Parameters**:

- `entryAddr` (`I64`)
- `Raw address of the entry point function. The function`
- `receives the thread ID as its first argument via rdi.`

#### `function getThreadId( self ) -> I64`

Return the unique identifier of this thread.

**Returns**: `I64` — The thread's numeric identifier.

#### `function getTask( self ) -> Task`

Return the underlying OS task for direct register manipulation or scheduler interaction.

**Returns**: `Task` — The OS-level task backing this thread.

#### `function getCompletionEvent( self ) -> Event`

Return the completion event that is signaled when this thread finishes execution.

**Returns**: `Event` — The kernel event used for completion notification.

#### `function setResult( self, I64 value ) -> Void`

Store a result value for this thread, to be retrieved by the joiner after the thread finishes.

**Parameters**:

- `value` (`I64`)
- `The result value to store.`

#### `function getResult( self ) -> I64`

Retrieve the result value stored by the thread.

**Returns**: `I64` — The result value, or 0 if no result was set.

#### `function markFinished( self ) -> Void`

Transition the thread to the Finished state and signal the completion event to wake any blocked joiners.

#### `function markPanicked( self ) -> Void`

Mark the thread as having panicked and transition to the Finished state. The panic flag can be checked by joiners to detect abnormal termination.

#### `function hasPanicked( self ) -> Boolean`

Check whether the thread terminated due to a panic.

**Returns**: `Boolean` — True if the thread panicked during execution.

#### `function isFinished( self ) -> Boolean`

Check whether the thread has finished execution.

**Returns**: `Boolean` — True if the thread is in the Finished or Detached state.

#### `function isDetached( self ) -> Boolean`

Check whether the thread has been detached from its owner.

**Returns**: `Boolean` — True if the thread is in the Detached state.

#### `function detach( self ) -> Void`

Transition the thread to the Detached state, indicating that no owner will join or clean up this thread.

#### `function cleanup( self ) -> Void`

Release all resources held by this thread: unregisters the task from the runtime, terminates the OS task, frees the stack memory, and destroys the result slot and atomic state variables.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

