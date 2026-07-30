# uranite.async.task

## Table of Contents

- [Imports](#imports)
- [const `SYS_MMAP`](#const-sys-mmap)
- [const `SYS_MUNMAP`](#const-sys-munmap)
- [const `PROT_RW`](#const-prot-rw)
- [const `MAP_PRIVATE_ANON`](#const-map-private-anon)
- [const `DEFAULT_STACK_SIZE`](#const-default-stack-size)
- [const `STATE_CREATED`](#const-state-created)
- [const `STATE_RUNNING`](#const-state-running)
- [const `STATE_SUSPENDED`](#const-state-suspended)
- [const `STATE_IO_WAITING`](#const-state-io-waiting)
- [const `STATE_COMPLETED`](#const-state-completed)
- [const `STATE_FAILED`](#const-state-failed)
- [const `FIELD_ID`](#const-field-id)
- [const `FIELD_STATE`](#const-field-state)
- [const `FIELD_ENTRY_FN`](#const-field-entry-fn)
- [const `FIELD_USER_DATA`](#const-field-user-data)
- [const `FIELD_STACK_BASE`](#const-field-stack-base)
- [const `FIELD_STACK_SIZE`](#const-field-stack-size)
- [const `FIELD_CONTEXT_RSP`](#const-field-context-rsp)
- [const `FIELD_RESULT`](#const-field-result)
- [const `FIELD_HAS_ERROR`](#const-field-has-error)
- [const `FIELD_AWAITING_ID`](#const-field-awaiting-id)
- [const `FIELD_AWAIT_FD`](#const-field-await-fd)
- [const `FIELD_AWAIT_EVENTS`](#const-field-await-events)
- [const `TASK_FIELD_COUNT`](#const-task-field-count)
- [function `createTask`](#function-createtask)
  - [`createTask()`](#createTask)
- [function `taskAllocateStack`](#function-taskallocatestack)
  - [`taskAllocateStack()`](#taskAllocateStack)
- [function `taskInitContext`](#function-taskinitcontext)
  - [`taskInitContext()`](#taskInitContext)
- [function `taskFreeStack`](#function-taskfreestack)
  - [`taskFreeStack()`](#taskFreeStack)
- [function `taskIsFinished`](#function-taskisfinished)
  - [`taskIsFinished()`](#taskIsFinished)
- [function `taskDestroy`](#function-taskdestroy)
  - [`taskDestroy()`](#taskDestroy)
- [function `taskRspAddr`](#function-taskrspaddr)
  - [`taskRspAddr()`](#taskRspAddr)

## Imports

- `uranite.async.context`
  - `makeContext`
- `uranite.async.errors`
  - `AsyncRuntimeError`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readI64At`
  - `writeI64At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall2`
  - `syscall6`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `SYS_MMAP`

Syscall number for mmap on x86-64 Linux. 

## const `SYS_MUNMAP`

Syscall number for munmap on x86-64 Linux. 

## const `PROT_RW`

Memory protection flags for read+write access (PROT_READ | PROT_WRITE). 

## const `MAP_PRIVATE_ANON`

Mapping flags for private anonymous memory (MAP_PRIVATE | MAP_ANONYMOUS). 

## const `DEFAULT_STACK_SIZE`

Default stack size in bytes (64 KiB) allocated for each task. 

## const `STATE_CREATED`

Task has been created but not yet started. 

## const `STATE_RUNNING`

Task is currently executing on the CPU. 

## const `STATE_SUSPENDED`

Task voluntarily yielded or is waiting for another task. 

## const `STATE_IO_WAITING`

Task is blocked waiting for I/O readiness via epoll. 

## const `STATE_COMPLETED`

Task has finished execution successfully. 

## const `STATE_FAILED`

Task terminated due to an error. 

## const `FIELD_ID`

Offset index for the task's unique identifier. 

## const `FIELD_STATE`

Offset index for the task's current state. 

## const `FIELD_ENTRY_FN`

Offset index for the task's entry function address. 

## const `FIELD_USER_DATA`

Offset index for user-defined data associated with the task. 

## const `FIELD_STACK_BASE`

Offset index for the base address of the task's stack. 

## const `FIELD_STACK_SIZE`

Offset index for the size of the task's stack in bytes. 

## const `FIELD_CONTEXT_RSP`

Offset index for the saved RSP value used in context switching. 

## const `FIELD_RESULT`

Offset index for the task's return value after completion. 

## const `FIELD_HAS_ERROR`

Offset index for the error flag (nonzero if the task failed). 

## const `FIELD_AWAITING_ID`

Offset index for the ID of the task this task is waiting on. 

## const `FIELD_AWAIT_FD`

Offset index for the file descriptor this task is waiting on. 

## const `FIELD_AWAIT_EVENTS`

Offset index for the epoll event mask this task is waiting for. 

## const `TASK_FIELD_COUNT`

Total number of I64 fields in the flat task structure. 

## function `createTask`

Allocate and initialize a new task structure as a flat Memory<I64> array. All fields are set to their initial values with the task in the created state.

**Parameters**:

- `id` (`I64`)
- `Unique identifier to assign to the task.`
- `entryFn` (`I64`)
- `Address of the function the task will execute.`
- `userData` (`I64`)
- `User-defined data associated with the task.`

**Returns**: — Memory<I64>:
The initialized task structure with TASK_FIELD_COUNT slots.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function createTask( I64 id, I64 entryFn, I64 userData ) -> Memory<I64>`

Allocate and initialize a new task structure as a flat Memory<I64> array. All fields are set to their initial values with the task in the created state.

**Parameters**:

- `id` (`I64`)
- `Unique identifier to assign to the task.`
- `entryFn` (`I64`)
- `Address of the function the task will execute.`
- `userData` (`I64`)
- `User-defined data associated with the task.`

**Returns**: — Memory<I64>:
The initialized task structure with TASK_FIELD_COUNT slots.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `taskAllocateStack`

Allocate a private anonymous memory region for the task's execution stack using the mmap syscall. The stack base address is stored in the task struct.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure to allocate a stack for.`

**Raises**:

- `AsyncRuntimeError` → `Error` — If the mmap syscall fails to allocate stack memory.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function taskAllocateStack( Memory<I64> task ) -> Void`

Allocate a private anonymous memory region for the task's execution stack using the mmap syscall. The stack base address is stored in the task struct.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure to allocate a stack for.`

**Raises**:

- `AsyncRuntimeError` → `Error` — If the mmap syscall fails to allocate stack memory.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `taskInitContext`

Prepare the task's execution context by constructing an initial register frame on its stack. The context will start execution at the trampoline function when first resumed.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure whose context to initialize. Must have its`
- `stack already allocated via taskAllocateStack.`
- `trampolineFn` (`I64`)
- `Address of the trampoline function that bootstraps task execution.`

### Methods

#### `function taskInitContext( Memory<I64> task, I64 trampolineFn ) -> Void`

Prepare the task's execution context by constructing an initial register frame on its stack. The context will start execution at the trampoline function when first resumed.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure whose context to initialize. Must have its`
- `stack already allocated via taskAllocateStack.`
- `trampolineFn` (`I64`)
- `Address of the trampoline function that bootstraps task execution.`

## function `taskFreeStack`

Release the task's stack memory back to the kernel via munmap. Resets the stack base to zero. Safe to call even if the stack was never allocated.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure whose stack to free.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function taskFreeStack( Memory<I64> task ) -> Void`

Release the task's stack memory back to the kernel via munmap. Resets the stack base to zero. Safe to call even if the stack was never allocated.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure whose stack to free.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `taskIsFinished`

Check whether a task has reached a terminal state (completed or failed).

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure to check.`

**Returns**: `Boolean` — True if the task is completed or failed, False otherwise.

### Methods

#### `function taskIsFinished( Memory<I64> task ) -> Boolean`

Check whether a task has reached a terminal state (completed or failed).

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure to check.`

**Returns**: `Boolean` — True if the task is completed or failed, False otherwise.

## function `taskDestroy`

Fully destroy a task by freeing its stack and releasing the task structure memory. The task must not be referenced after this call.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure to destroy.`

### Methods

#### `function taskDestroy( Memory<I64> task ) -> Void`

Fully destroy a task by freeing its stack and releasing the task structure memory. The task must not be referenced after this call.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure to destroy.`

## function `taskRspAddr`

Compute the raw memory address of the task's CONTEXT_RSP field, suitable for passing directly to swapContext.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure whose RSP address to compute.`

**Returns**: — I64:
The memory address of the FIELD_CONTEXT_RSP slot in the task.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function taskRspAddr( Memory<I64> task ) -> I64`

Compute the raw memory address of the task's CONTEXT_RSP field, suitable for passing directly to swapContext.

**Parameters**:

- `task` (`Memory<I64>`)
- `The task structure whose RSP address to compute.`

**Returns**: — I64:
The memory address of the FIELD_CONTEXT_RSP slot in the task.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

