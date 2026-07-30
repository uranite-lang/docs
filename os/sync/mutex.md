# uranite.os.sync.mutex

## Table of Contents

- [Imports](#imports)
- [class `KernelMutex`](#class-kernelmutex)
  - [`KernelMutex()`](#KernelMutex)
  - [`acquire()`](#acquire)
  - [`release()`](#release)
  - [`isHeld()`](#isHeld)
  - [`getOwner()`](#getOwner)
  - [`getWaitCount()`](#getWaitCount)

## Imports

- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `KernelMutex`

Sleep-based kernel mutex for mutual exclusion between tasks. When contended, the calling task blocks instead of spinning, which requires the scheduler for block and unblock operations. A spinlock protects the internal state and is held only briefly.

### Fields

| Name | Type | Access |
|------|------|--------|
| `guard` | `Spinlock` | protect |
| `owner` | `I64` | protect |
| `held` | `Boolean` | protect |
| `waitCount` | `I64` | protect |

### Methods

#### `function KernelMutex( self ) -> Void`

Construct an unheld kernel mutex with no owner and zero waiters. The internal guard spinlock is initialized for protecting state transitions.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function acquire( self, I64 taskId ) -> Boolean`

Attempt to acquire the mutex for the given task. If the mutex is not held, it is acquired immediately and returns True. If already held by another task, the wait count is incremented and returns False. The caller is responsible for blocking the current task via the scheduler when False is returned.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function release( self, I64 taskId ) -> Boolean`

Release the mutex held by the given task. Returns True if there are waiting tasks, indicating the caller should wake one of them. Returns False if the releasing task does not own the mutex or if there are no waiters.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isHeld( self ) -> Boolean`

Return whether the mutex is currently held by any task. 

#### `function getOwner( self ) -> I64`

Return the task ID of the current mutex owner, or 0 if unheld. 

#### `function getWaitCount( self ) -> I64`

Return the number of tasks currently waiting to acquire this mutex. 

