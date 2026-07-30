# uranite.process.process

## Table of Contents

- [Imports](#imports)
- [const `DEFAULT_PROCESS_STACK_PAGES`](#const-default-process-stack-pages)
- [const `PAGE_SIZE`](#const-page-size)
- [const `DEFAULT_PROCESS_PRIORITY`](#const-default-process-priority)
- [const `PROCESS_STATE_NEW`](#const-process-state-new)
- [const `PROCESS_STATE_RUNNING`](#const-process-state-running)
- [const `PROCESS_STATE_FINISHED`](#const-process-state-finished)
- [const `PROCESS_STATE_CRASHED`](#const-process-state-crashed)
- [const `PROCESS_STATE_DETACHED`](#const-process-state-detached)
- [const `SHARED_MEM_FLAGS_RW`](#const-shared-mem-flags-rw)
- [class `ProcessRuntime`](#class-processruntime)
  - [`ProcessRuntime()`](#ProcessRuntime)
  - [`allocateStack()`](#allocateStack)
  - [`freeStack()`](#freeStack)
  - [`allocateProcessId()`](#allocateProcessId)
  - [`createAddressSpace()`](#createAddressSpace)
  - [`createSharedRegion()`](#createSharedRegion)
  - [`mapSharedRegion()`](#mapSharedRegion)
  - [`registerTask()`](#registerTask)
  - [`lookupTaskPointer()`](#lookupTaskPointer)
  - [`unregisterTask()`](#unregisterTask)
  - [`getScheduler()`](#getScheduler)
  - [`getPmm()`](#getPmm)
  - [`getClock()`](#getClock)
  - [`getSharedRegistry()`](#getSharedRegistry)
  - [`destroy()`](#destroy)
- [class `Process`](#class-process)
  - [`Process()`](#Process)
  - [`start()`](#start)
  - [`getProcessId()`](#getProcessId)
  - [`getTask()`](#getTask)
  - [`getAddressSpace()`](#getAddressSpace)
  - [`getCompletionEvent()`](#getCompletionEvent)
  - [`setResult()`](#setResult)
  - [`getResult()`](#getResult)
  - [`markFinished()`](#markFinished)
  - [`markCrashed()`](#markCrashed)
  - [`hasCrashed()`](#hasCrashed)
  - [`isFinished()`](#isFinished)
  - [`isDetached()`](#isDetached)
  - [`detach()`](#detach)
  - [`cleanup()`](#cleanup)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.ipc.shared-memory`
  - `SharedMemoryRegistry`
  - `SharedRegion`
- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.memory.pmm`
  - `PhysicalMemoryManager`
- `uranite.os.memory.address-space`
  - `AddressSpace`
  - `protReadWrite`
  - `protUser`
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
- `uranite.process.errors`
  - `ProcessCreationError`
  - `ProcessError`
- `uranite.threading.atomic`
  - `AtomicBoolean`
  - `AtomicI64`

## const `DEFAULT_PROCESS_STACK_PAGES`

Default number of pages allocated for a process stack (64 KiB at 4096 bytes per page). 

## const `PAGE_SIZE`

Size of a single memory page in bytes. 

## const `DEFAULT_PROCESS_PRIORITY`

Default scheduler priority assigned to newly created processes. 

## const `PROCESS_STATE_NEW`

Process state: created but not yet started. 

## const `PROCESS_STATE_RUNNING`

Process state: actively executing on the scheduler. 

## const `PROCESS_STATE_FINISHED`

Process state: execution completed normally. 

## const `PROCESS_STATE_CRASHED`

Process state: execution terminated due to an unhandled error. 

## const `PROCESS_STATE_DETACHED`

Process state: detached from its handle, resources freed on completion. 

## const `SHARED_MEM_FLAGS_RW`

Shared memory flags for read-write access. 

## class `ProcessRuntime`

Manages kernel subsystem access for process creation and lifecycle.

Provides a unified interface to the scheduler, physical memory manager, system clock, and shared memory registry. Handles process ID allocation, stack allocation/deallocation, address space creation, shared memory region management, and task pointer registration. A single ProcessRuntime instance is shared across all processes within an executor or pool.

### Fields

| Name | Type | Access |
|------|------|--------|
| `scheduler` | `Scheduler` | protect |
| `pmm` | `PhysicalMemoryManager` | protect |
| `clock` | `SystemClock` | protect |
| `sharedRegistry` | `SharedMemoryRegistry` | protect |
| `guard` | `Spinlock` | protect |
| `nextProcessId` | `AtomicI64` | protect |
| `initialized` | `AtomicBoolean` | protect |
| `taskPointers` | `Memory<I64>` | protect |
| `maxProcesses` | `I64` | protect |

### Methods

#### `function ProcessRuntime( self, Scheduler scheduler, PhysicalMemoryManager pmm, SystemClock clock, I64 maxProcesses ) -> Void`

Construct a new ProcessRuntime with the given kernel subsystems.

Initializes the shared memory registry, process ID counter, task pointer table, and synchronization primitives. The task pointer table is zeroed to indicate no registered tasks.

**Parameters**:

- `scheduler` (`Scheduler`)
- `The task scheduler for process execution.`
- `pmm` (`PhysicalMemoryManager`)
- `The physical memory manager for stack and region allocation.`
- `clock` (`SystemClock`)
- `The system clock for time-related operations.`
- `maxProcesses` (`I64`)
- `Maximum number of concurrent processes supported.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function allocateStack( self, I64 stackPages ) -> I64`

Allocate a contiguous physical memory region for a process stack.

Allocates one extra page beyond the requested count to serve as a guard page for stack overflow detection.

**Parameters**:

- `stackPages` (`I64`)
- `Number of usable stack pages to allocate.`

**Returns**: `I64` — The physical base address of the allocated stack region.

#### `function freeStack( self, I64 stackBase, I64 stackPages ) -> Void`

Free a previously allocated stack region.

**Parameters**:

- `stackBase` (`I64`)
- `The physical base address returned by allocateStack.`
- `stackPages` (`I64`)
- `The number of usable stack pages` (`same value passed to allocateStack`)

#### `function allocateProcessId( self ) -> I64`

Atomically allocate and return a unique process identifier.

**Returns**: `I64` — A monotonically increasing process ID, starting from 1.

#### `function createAddressSpace( self ) -> AddressSpace`

Create a new virtual address space for a process.

**Returns**: — AddressSpace:
A fresh address space backed by this runtime's physical
memory manager.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function createSharedRegion( self, I64 ownerTaskId, I64 pageCount ) -> SharedRegion`

Allocate a shared memory region and register it in the shared registry.

The region is backed by contiguous physical pages and is assigned a unique region ID. It is created with read-write access flags.

**Parameters**:

- `ownerTaskId` (`I64`)
- `The task ID of the process that owns this region.`
- `pageCount` (`I64`)
- `Number of physical pages to allocate for the region.`

**Returns**: — SharedRegion:
The newly created and registered shared memory region.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function mapSharedRegion( self, AddressSpace addrSpace, SharedRegion region, I64 virtAddr ) -> Boolean`

Map an existing shared memory region into a process address space.

Creates a virtual-to-physical mapping for the region's pages at the specified virtual address. Increments the region's reference count on success.

**Parameters**:

- `addrSpace` (`AddressSpace`)
- `The target address space to map into.`
- `region` (`SharedRegion`)
- `The shared memory region to map.`
- `virtAddr` (`I64`)
- `The virtual address where the region should appear.`

**Returns**: — Boolean:
True if the mapping succeeded, False on failure.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function registerTask( self, I64 taskId, I64 taskPointer ) -> Void`

Register a task pointer for a given task ID in the lookup table.

Thread-safe via spinlock. Only registers if the task ID is within the valid range.

**Parameters**:

- `taskId` (`I64`)
- `The task identifier to register.`
- `taskPointer` (`I64`)
- `The raw pointer to the task, cast to I64.`

#### `function lookupTaskPointer( self, I64 taskId ) -> I64`

Look up the task pointer for a given task ID.

**Parameters**:

- `taskId` (`I64`)
- `The task identifier to look up.`

**Returns**: `I64` — The raw task pointer, or 0 if the task ID is out of range or not registered.

#### `function unregisterTask( self, I64 taskId ) -> Void`

Remove a task pointer registration for a given task ID.

Thread-safe via spinlock. Sets the slot to 0 to indicate the task is no longer registered.

**Parameters**:

- `taskId` (`I64`)
- `The task identifier to unregister.`

#### `function getScheduler( self ) -> Scheduler`

Return the scheduler used by this runtime.

**Returns**: `Scheduler` — The task scheduler instance.

#### `function getPmm( self ) -> PhysicalMemoryManager`

Return the physical memory manager used by this runtime.

**Returns**: `PhysicalMemoryManager` — The physical memory manager instance.

#### `function getClock( self ) -> SystemClock`

Return the system clock used by this runtime.

**Returns**: `SystemClock` — The system clock instance.

#### `function getSharedRegistry( self ) -> SharedMemoryRegistry`

Return the shared memory registry used by this runtime.

**Returns**: `SharedMemoryRegistry` — The shared memory registry instance.

#### `function destroy( self ) -> Void`

Release all resources held by this runtime.

Frees the task pointer table, destroys atomic counters, and tears down the shared memory registry. Must be called after all processes managed by this runtime have been cleaned up.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Process`

An isolated execution context with its own address space and stack.

Represents a single process backed by a kernel Task. Each process has a private address space, a physical stack region, a completion event for join synchronization, and atomic state tracking. Processes are created through a ProcessRuntime and scheduled via its Scheduler.

### Fields

| Name | Type | Access |
|------|------|--------|
| `task` | `Task` | protect |
| `processId` | `I64` | protect |
| `stackBasePhys` | `I64` | protect |
| `stackPages` | `I64` | protect |
| `runtime` | `ProcessRuntime` | protect |
| `addressSpace` | `AddressSpace` | protect |
| `completionEvent` | `Event` | protect |
| `state` | `AtomicI64` | protect |
| `resultSlot` | `Memory<I64>` | protect |
| `crashed` | `AtomicBoolean` | protect |

### Methods

#### `function Process( self, ProcessRuntime runtime, I64 processId, I64 stackPages, I64 priority ) -> Void`

Construct a new process with the given runtime, identity, and resources.

Allocates a stack, creates a private address space, initializes a kernel Task with the specified priority, and registers the task in the runtime's lookup table. The process starts in the NEW state with its instruction pointer set to 0 (must be set before starting).

**Parameters**:

- `runtime` (`ProcessRuntime`)
- `The runtime providing kernel subsystem access.`
- `processId` (`I64`)
- `Unique identifier for this process.`
- `stackPages` (`I64`)
- `Number of stack pages to allocate.`
- `priority` (`I64`)
- `Scheduler priority for the backing task.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function start( self, <type> entry ) -> Void`

Start the process by setting its entry point and enqueuing it.

Extracts the function address from the callable, sets the instruction pointer, passes the process ID as the first argument (rdi), transitions to running state, and enqueues the backing task.

**Parameters**:

- `entry` (`Callable<Void, <>>`)
- `The function to execute as the process entry point.`

#### `function startRaw( self, I64 entryAddr ) -> Void`

Start the process from a raw function address. Internal use.

**Parameters**:

- `entryAddr` (`I64`)
- `The address of the function to execute.`

#### `function getProcessId( self ) -> I64`

Return the unique identifier of this process.

**Returns**: `I64` — The process ID assigned at construction.

#### `function getTask( self ) -> Task`

Return the kernel task backing this process.

**Returns**: `Task` — The underlying task object.

#### `function getAddressSpace( self ) -> AddressSpace`

Return the virtual address space private to this process.

**Returns**: `AddressSpace` — The process's address space.

#### `function getCompletionEvent( self ) -> Event`

Return the event that is signaled when this process finishes.

**Returns**: `Event` — The completion event for join synchronization.

#### `function setResult( self, I64 value ) -> Void`

Store the process return value.

**Parameters**:

- `value` (`I64`)
- `The result value produced by the process.`

#### `function getResult( self ) -> I64`

Retrieve the process return value.

Should only be called after the process has finished.

**Returns**: `I64` — The result value stored by setResult, or 0 if none was set.

#### `function markFinished( self ) -> Void`

Transition the process to the finished state and signal waiters.

Sets the state to PROCESS_STATE_FINISHED and signals the completion event to unblock any tasks waiting on join.

#### `function markCrashed( self ) -> Void`

Mark the process as crashed and signal waiters.

Sets the crashed flag and then transitions to finished state, signaling the completion event so that joining tasks can detect the crash.

#### `function hasCrashed( self ) -> Boolean`

Check whether this process terminated due to a crash.

**Returns**: `Boolean` — True if the process crashed, False otherwise.

#### `function isFinished( self ) -> Boolean`

Check whether this process has reached a terminal state.

A process is finished when its state is PROCESS_STATE_FINISHED, PROCESS_STATE_CRASHED, or PROCESS_STATE_DETACHED.

**Returns**: `Boolean` — True if the process is in a terminal state.

#### `function isDetached( self ) -> Boolean`

Check whether this process has been detached from its handle.

**Returns**: `Boolean` — True if the process is in the detached state.

#### `function detach( self ) -> Void`

Detach the process from its handle.

A detached process's resources are freed automatically upon completion rather than requiring an explicit join.

#### `function cleanup( self ) -> Void`

Release all resources held by this process.

Unregisters the task, terminates it, frees the stack memory, and destroys the result slot and atomic state. Must be called exactly once after the process has finished or been joined.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

