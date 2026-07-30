# uranite.threading.mutex

## Table of Contents

- [Imports](#imports)
- [class `MutexGuard`](#class-mutexguard)
  - [`MutexGuard()`](#MutexGuard)
  - [`get()`](#get)
  - [`set()`](#set)
  - [`release()`](#release)
- [class `Mutex`](#class-mutex)
  - [`Mutex()`](#Mutex)
  - [`lock()`](#lock)
  - [`tryLock()`](#tryLock)
  - [`getData()`](#getData)
  - [`setData()`](#setData)
  - [`unlockInternal()`](#unlockInternal)
  - [`poison()`](#poison)
  - [`isPoisoned()`](#isPoisoned)
  - [`withLock()`](#withLock)
  - [`destroy()`](#destroy)

## Imports

- `uranite.language.callable`
  - `Callable`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.sync.mutex`
  - `KernelMutex`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.threading.atomic`
  - `AtomicBoolean`
- `uranite.threading.errors`
  - `PoisonError`

## class `MutexGuard`<T>

Scoped access token for a Mutex<T>. Obtained from Mutex.lock() or Mutex.tryLock(), a MutexGuard grants exclusive read and write access to the protected data. The guard must be explicitly released via release() to unlock the mutex -- there is no automatic RAII cleanup.

### Fields

| Name | Type | Access |
|------|------|--------|
| `mutex` | `Mutex<T>` | protect |
| `ownerId` | `I64` | protect |

### Methods

#### `function MutexGuard( self, Mutex<T> mutex, I64 ownerId ) -> Void`

Create a new guard bound to the given mutex and owner.

**Parameters**:

- `mutex` (`Mutex<T>`)
- `The mutex this guard provides access to.`
- `ownerId` (`I64`)
- `The task identifier that acquired the lock.`

#### `function get( self ) -> T`

Read the protected data.

**Returns**: `T` — The current value of the data protected by the mutex.

#### `function set( self, T value ) -> Void`

Write a new value to the protected data.

**Parameters**:

- `value` (`T`)
- `The new value to store.`

#### `function release( self ) -> Void`

Release the lock, allowing other tasks to acquire the mutex. After calling release, this guard must not be used again.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Mutex`<T>

Data-protecting mutual exclusion lock. Owns the protected data -- the value is only accessible through a MutexGuard obtained from lock() or tryLock(). Supports poison detection: if a thread panics while holding the lock, subsequent lock attempts will raise a PoisonError.

Built on KernelMutex from the OS sync module with cooperative blocking through the scheduler when contention occurs.

### Fields

| Name | Type | Access |
|------|------|--------|
| `inner` | `KernelMutex` | protect |
| `data` | `T` | protect |
| `scheduler` | `Scheduler` | protect |
| `poisoned` | `AtomicBoolean` | protect |

### Methods

#### `function Mutex( self, T value, Scheduler scheduler ) -> Void`

Create a new mutex protecting the given initial value.

**Parameters**:

- `value` (`T`)
- `The initial value to protect.`
- `scheduler` (`Scheduler`)
- `The scheduler to use for cooperative blocking when the mutex`
- `is contended.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function lock( self ) -> MutexGuard<T>`

Acquire the mutex, blocking the current task until the lock is available. Uses cooperative blocking through the scheduler, yielding the current task when the lock cannot be immediately acquired.

**Returns**: `MutexGuard<T>` — A guard granting exclusive access to the protected data.

**Raises**:

- `PoisonError` — If the mutex was poisoned by a panicked lock holder.

**Complexity**:
- Time: `O(1) amortized, unbounded under contention`
- Space: `O(1)`

#### `function tryLock( self ) -> ?MutexGuard<T>`

Attempt to acquire the mutex without blocking. Returns immediately whether or not the lock was acquired.

**Returns**: — ?MutexGuard<T>:
A guard granting exclusive access if the lock was acquired,
or None if the mutex is already held or poisoned.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getData( self ) -> T`

Return the raw protected data. Intended for internal use by MutexGuard only -- callers should access data through the guard.

**Returns**: `T` — The current value of the protected data.

#### `function setData( self, T value ) -> Void`

Replace the protected data. Intended for internal use by MutexGuard only -- callers should modify data through the guard.

**Parameters**:

- `value` (`T`)
- `The new value to store.`

#### `function unlockInternal( self, I64 taskId ) -> Void`

Release the kernel mutex and yield the current task to allow other waiters to acquire the lock. Called by MutexGuard.release().

**Parameters**:

- `taskId` (`I64`)
- `The task identifier of the current lock holder.`

#### `function poison( self ) -> Void`

Mark the mutex as poisoned. Subsequent lock attempts will raise a PoisonError. Typically called when a thread panics while holding the lock.

#### `function isPoisoned( self ) -> Boolean`

Check whether the mutex has been poisoned.

**Returns**: `Boolean` — True if the mutex was poisoned by a panicked holder.

#### `function withLock( self, <type> criticalSection ) -> Void`

Acquire the mutex, execute the critical section callback with the protected data, and release the lock. Guarantees unlock on all exit paths including exceptions.

**Parameters**:

- `criticalSection` (`Callable<Void, <T>>`)
- `A callback receiving the protected data value.`

**Raises**:

- `PoisonError` — If the mutex is poisoned.

**Complexity**:
- Time: `O(1) for lock/unlock, plus O(criticalSection)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release resources held by the mutex's poison flag.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

