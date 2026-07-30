# uranite.threading.join-handle

## Table of Contents

- [Imports](#imports)
- [class `JoinHandle`](#class-joinhandle)
  - [`JoinHandle()`](#JoinHandle)
  - [`join()`](#join)
  - [`isFinished()`](#isFinished)
  - [`getThreadId()`](#getThreadId)
  - [`detach()`](#detach)

## Imports

- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.threading.errors`
  - `ThreadError`
  - `ThreadPoisonError`
- `uranite.threading.thread`
  - `Thread`
  - `ThreadRuntime`

## class `JoinHandle`

Ownership handle for a spawned thread. Enforces that every thread is either joined or detached exactly once. Joining blocks the caller until the thread finishes, retrieves its result, and performs stack cleanup. Detaching releases ownership so the thread runs independently.

Exactly one JoinHandle exists per Thread. Attempting to join a thread that has already been joined or detached raises a ThreadError.

### Fields

| Name | Type | Access |
|------|------|--------|
| `thread` | `Thread` | protect |
| `runtime` | `ThreadRuntime` | protect |
| `joined` | `Boolean` | protect |

### Methods

#### `function JoinHandle( self, Thread thread, ThreadRuntime runtime ) -> Void`

Create a new join handle owning the given thread.

**Parameters**:

- `thread` (`Thread`)
- `The thread to take ownership of.`
- `runtime` (`ThreadRuntime`)
- `The thread runtime used for cooperative blocking during join.`

#### `function join( self ) -> I64`

Block the calling task until the owned thread finishes, then return its result. Performs thread cleanup (stack deallocation, task unregistration) before returning. If the thread panicked, cleanup is still performed but a ThreadPoisonError is raised.

**Returns**: `I64` — The result value set by the thread before it finished.

**Raises**:

- `ThreadError` → `Error` — If the thread has already been joined or detached.
- `ThreadPoisonError` → `ThreadError` → `Error` — If the thread terminated due to a panic.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isFinished( self ) -> Boolean`

Check whether the owned thread has finished execution without blocking.

**Returns**: `Boolean` — True if the thread has reached the Finished or Detached state.

#### `function getThreadId( self ) -> I64`

Return the unique identifier of the owned thread.

**Returns**: `I64` — The thread's numeric identifier.

#### `function detach( self ) -> Void`

Release ownership of the thread, allowing it to run independently. The thread's resources will not be cleaned up by this handle after detaching. Cannot be called after join.

**Raises**:

- `ThreadError` → `Error` — If the thread has already been joined.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

