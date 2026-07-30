# uranite.os.sync.semaphore

## Table of Contents

- [Imports](#imports)
- [class `Semaphore`](#class-semaphore)
  - [`Semaphore()`](#Semaphore)
  - [`tryWait()`](#tryWait)
  - [`signal()`](#signal)
  - [`getCount()`](#getCount)
  - [`getWaitCount()`](#getWaitCount)

## Imports

- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `Semaphore`

Counting semaphore for managing access to a limited pool of resources. The count represents the number of available resource slots. tryWait() decrements the count and returns True if a slot is available, or increments the wait count and returns False if the caller must block. signal() increments the count and returns True if waiters exist, indicating the caller should wake one blocked task.

### Fields

| Name | Type | Access |
|------|------|--------|
| `guard` | `Spinlock` | protect |
| `count` | `I64` | protect |
| `waitCount` | `I64` | protect |

### Methods

#### `function Semaphore( self, I64 initial ) -> Void`

Construct a semaphore with the given initial count representing the number of available resource slots. The wait count starts at zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function tryWait( self ) -> Boolean`

Attempt to decrement the semaphore count by one. Returns True if a resource slot was available and successfully acquired. Returns False if the count was zero, in which case the wait count is incremented and the caller should block the current task via the scheduler.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function signal( self ) -> Boolean`

Increment the semaphore count by one, releasing a resource slot. Returns True if there are waiting tasks, indicating the caller should wake one of them. Returns False if there are no waiters.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Return the current number of available resource slots. 

#### `function getWaitCount( self ) -> I64`

Return the number of tasks currently waiting for a resource slot. 

