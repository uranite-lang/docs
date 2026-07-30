# uranite.threading.condvar

## Table of Contents

- [Imports](#imports)
- [class `CondVar`](#class-condvar)
  - [`CondVar()`](#CondVar)
  - [`wait()`](#wait)
  - [`notifyOne()`](#notifyOne)
  - [`notifyAll()`](#notifyAll)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.sync.spinlock`
  - `Spinlock`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.threading.mutex`
  - `MutexGuard`
- `uranite.threading.thread`
  - `ThreadRuntime`

## class `CondVar`

Condition variable that allows threads to wait until a condition is signaled by another thread. Maintains a bounded circular wait queue of task identifiers. The caller must hold a MutexGuard when calling wait(); the guard is released atomically before blocking and must be re-acquired by the caller after waking.

### Fields

| Name | Type | Access |
|------|------|--------|
| `guard` | `Spinlock` | protect |
| `waitQueue` | `Memory<I64>` | protect |
| `waitHead` | `I64` | protect |
| `waitTail` | `I64` | protect |
| `waitCount` | `I64` | protect |
| `waitCapacity` | `I64` | protect |
| `scheduler` | `Scheduler` | protect |
| `runtime` | `ThreadRuntime` | protect |

### Methods

#### `function CondVar( self, I64 maxWaiters, Scheduler scheduler, ThreadRuntime runtime ) -> Void`

Construct a condition variable with a fixed-capacity wait queue.

**Parameters**:

- `maxWaiters` (`I64`)
- `Maximum number of threads that can wait simultaneously.`
- `scheduler` (`Scheduler`)
- `Scheduler for cooperative task blocking.`
- `runtime` (`ThreadRuntime`)
- `Runtime for task pointer lookups during notification.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function wait( self, MutexGuard<I64> guard ) -> Void`

Atomically release the mutex guard and block the current task until signaled by another thread via notifyOne or notifyAll. The caller must re-acquire the mutex after this method returns.

**Parameters**:

- `guard` (`MutexGuard<I64>`)
- `The mutex guard to release before blocking. The guard is`
- `released inside this method.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function notifyOne( self ) -> Void`

Wake one waiting thread, if any. Removes the oldest waiter from the queue and yields the scheduler to allow it to run.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function notifyAll( self ) -> Void`

Wake all waiting threads. Drains the entire wait queue and yields the scheduler so all woken threads can be rescheduled.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release the wait queue memory. The condition variable must not be used after this call.

