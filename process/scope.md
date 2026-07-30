# uranite.process.scope

## Table of Contents

- [Imports](#imports)
- [const `MAX_SCOPED_PROCESSES`](#const-max-scoped-processes)
- [class `ScopedProcess`](#class-scopedprocess)
  - [`ScopedProcess()`](#ScopedProcess)
  - [`spawn()`](#spawn)
  - [`joinAll()`](#joinAll)
  - [`getProcessCount()`](#getProcessCount)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.process.errors`
  - `ProcessError`
- `uranite.process.process`
  - `DEFAULT_PROCESS_PRIORITY`
  - `DEFAULT_PROCESS_STACK_PAGES`
  - `Process`
  - `ProcessRuntime`
- `uranite.process.process-handle`
  - `ProcessHandle`

## const `MAX_SCOPED_PROCESSES`

Maximum number of processes that can be spawned within a single scope. 

## class `ScopedProcess`

Structured concurrency scope that ensures no spawned process outlives it.

Collects all processes spawned within the scope and provides a joinAll method that blocks until every process has completed, cleaning up all resources. If any process crashes during joinAll, a ProcessError is raised after all remaining processes have been joined.

### Fields

| Name | Type | Access |
|------|------|--------|
| `handles` | `Memory<ProcessHandle>` | protect |
| `count` | `I64` | protect |
| `capacity` | `I64` | protect |
| `runtime` | `ProcessRuntime` | protect |

### Methods

#### `function ScopedProcess( self, ProcessRuntime runtime ) -> Void`

Construct a new process scope backed by the given runtime.

Allocates storage for up to MAX_SCOPED_PROCESSES handles.

**Parameters**:

- `runtime` (`ProcessRuntime`)
- `The runtime providing kernel subsystem access for`
- `process creation and scheduling.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function spawn( self, I64 entryAddr ) -> I64`

Spawn a new process within this scope.

Creates a process with default stack size and priority, starts it at the given entry address, and registers a handle for later join. The process receives its process ID as the first argument (rdi).

**Parameters**:

- `entryAddr` (`I64`)
- `The address of the function to execute as the process`
- `entry point.`

**Returns**: `I64` — The unique process ID of the newly spawned process.

**Raises**:

- `ProcessError` → `Error` — If the scope has reached its maximum process capacity.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function joinAll( self ) -> Void`

Block until all spawned processes have completed and clean up resources.

Joins every process in the scope in order of creation. If any process crashes, the remaining processes are still joined before raising a ProcessError. The handle array is freed regardless of outcome.

**Raises**:

- `ProcessError` → `Error` — If one or more scoped processes crashed during execution.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function getProcessCount( self ) -> I64`

Return the number of processes currently tracked by this scope.

**Returns**: `I64` — The count of spawned processes.

