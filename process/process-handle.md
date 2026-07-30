# uranite.process.process-handle

## Table of Contents

- [Imports](#imports)
- [class `ProcessHandle`](#class-processhandle)
  - [`ProcessHandle()`](#ProcessHandle)
  - [`join()`](#join)
  - [`isFinished()`](#isFinished)
  - [`getProcessId()`](#getProcessId)
  - [`detach()`](#detach)

## Imports

- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.process.errors`
  - `ProcessCrashedError`
  - `ProcessError`
- `uranite.process.process`
  - `Process`
  - `ProcessRuntime`

## class `ProcessHandle`

Ownership handle for a Process that enforces cleanup on join or detach.

Provides RAII-style lifecycle management for a single Process. Exactly one ProcessHandle exists per Process. Calling join blocks until the process finishes, retrieves its result, and cleans up its resources. Calling detach releases ownership so the process runs independently. A process can only be joined or detached once.

### Fields

| Name | Type | Access |
|------|------|--------|
| `process` | `Process` | protect |
| `runtime` | `ProcessRuntime` | protect |
| `joined` | `Boolean` | protect |

### Methods

#### `function ProcessHandle( self, Process process, ProcessRuntime runtime ) -> Void`

Construct a new handle taking ownership of the given process.

**Parameters**:

- `process` (`Process`)
- `The process to own and manage.`
- `runtime` (`ProcessRuntime`)
- `The runtime that created the process.`

#### `function join( self ) -> I64`

Block until the process finishes and return its result.

Waits for the process to reach a terminal state using cooperative blocking via the scheduler. After completion, cleans up all process resources. Raises ProcessCrashedError if the process crashed, or ProcessError if join is called more than once.

**Returns**: `I64` — The result value produced by the process.

**Raises**:

- `ProcessError` → `Error` — If this handle has already been joined or detached.
- `ProcessCrashedError` → `ProcessError` → `Error` — If the process terminated due to a crash.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isFinished( self ) -> Boolean`

Check whether the owned process has reached a terminal state.

**Returns**: `Boolean` — True if the process is finished, crashed, or detached.

#### `function getProcessId( self ) -> I64`

Return the process ID of the owned process.

**Returns**: `I64` — The unique process identifier.

#### `function detach( self ) -> Void`

Release ownership of the process, allowing it to run independently.

After detaching, the process's resources will be freed automatically upon completion. The handle becomes unusable after this call.

**Raises**:

- `ProcessError` → `Error` — If this handle has already been joined or detached.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

