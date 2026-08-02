# uranite.threading.scope

## Table of Contents

- [Imports](#imports)
- [const `MAX_SCOPED_THREADS`](#const-max-scoped-threads)
- [class `ScopedThread`](#class-scopedthread)
  - [`ScopedThread()`](#ScopedThread)
  - [`spawn()`](#spawn)
  - [`joinAll()`](#joinAll)
  - [`getThreadCount()`](#getThreadCount)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.threading.errors`
  - `ThreadPoisonError`
  - `ThreadError`
- `uranite.threading.join-handle`
  - `JoinHandle`
- `uranite.threading.thread`
  - `DEFAULT_PRIORITY`
  - `DEFAULT_STACK_PAGES`
  - `Thread`
  - `ThreadRuntime`

## const `MAX_SCOPED_THREADS`

Maximum number of threads that can be spawned within a single scope.

## class `ScopedThread`

Structured concurrency scope that collects spawned threads and joins all of them when the scope exits. Guarantees that no thread outlives the scope -- joinAll must be called to clean up all spawned threads.

Threads are spawned with default stack size and priority. Each spawned thread is assigned a JoinHandle that is stored internally and joined during joinAll. If any thread panics, joinAll still joins all remaining threads before raising a ThreadError.

### Fields

| Name | Type | Access |
|------|------|--------|
| `handles` | `Memory<JoinHandle>` | protect |
| `count` | `I64` | protect |
| `capacity` | `I64` | protect |
| `runtime` | `ThreadRuntime` | protect |

### Methods

#### `function ScopedThread( self, ThreadRuntime runtime ) -> Void`

Create a new scoped thread container with capacity for up to MAX_SCOPED_THREADS threads.

**Parameters**:

- `runtime` (`ThreadRuntime`)
- `The thread runtime used to allocate and schedule threads.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function spawn( self, I64 entryAddr ) -> I64`

Spawn a new thread within this scope. The thread begins execution at the given function address with the thread ID passed in rdi.

**Parameters**:

- `entryAddr` (`I64`)
- `Raw address of the entry point function. The function`
- `receives the thread ID as its first argument.`

**Returns**: `I64` — The unique identifier assigned to the spawned thread.

**Raises**:

- `ThreadError` → `Error` — If the scope has reached its maximum thread capacity.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function joinAll( self ) -> Void`

Join all spawned threads, blocking until every thread has finished. Frees the internal handle storage after all threads are joined. If one or more threads panicked, a ThreadError is raised after all threads have been joined and cleaned up.

**Raises**:

- `ThreadError` → `Error` — If any spawned thread terminated due to a panic.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function getThreadCount( self ) -> I64`

Return the number of threads currently spawned in this scope.

**Returns**: `I64` — The count of spawned threads.

