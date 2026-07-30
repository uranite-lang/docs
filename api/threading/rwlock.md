# uranite.threading.rwlock

## Table of Contents

- [Imports](#imports)
- [class `ReadGuard`](#class-readguard)
  - [`ReadGuard()`](#ReadGuard)
  - [`get()`](#get)
  - [`release()`](#release)
- [class `WriteGuard`](#class-writeguard)
  - [`WriteGuard()`](#WriteGuard)
  - [`get()`](#get)
  - [`set()`](#set)
  - [`release()`](#release)
- [class `RwLock`](#class-rwlock)
  - [`RwLock()`](#RwLock)
  - [`readLock()`](#readLock)
  - [`writeLock()`](#writeLock)
  - [`tryReadLock()`](#tryReadLock)
  - [`tryWriteLock()`](#tryWriteLock)
  - [`getData()`](#getData)
  - [`setData()`](#setData)
  - [`readUnlockInternal()`](#readUnlockInternal)
  - [`writeUnlockInternal()`](#writeUnlockInternal)
  - [`poison()`](#poison)
  - [`isPoisoned()`](#isPoisoned)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.sync.read-write-lock`
  - `ReadWriteLock`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.threading.atomic`
  - `AtomicBoolean`
- `uranite.threading.errors`
  - `PoisonError`

## class `ReadGuard`<T>

Shared read access token for an RwLock<T>. Multiple ReadGuards can coexist simultaneously, allowing concurrent readers. The guard provides read-only access to the protected data and must be explicitly released via release() to decrement the reader count.

### Fields

| Name | Type | Access |
|------|------|--------|
| `lock` | `RwLock<T>` | protect |

### Methods

#### `function ReadGuard( self, RwLock<T> lock ) -> Void`

Create a new read guard bound to the given reader-writer lock.

**Parameters**:

- `lock` (`RwLock<T>`)
- `The reader-writer lock this guard provides access to.`

#### `function get( self ) -> T`

Read the protected data.

**Returns**: `T` — The current value of the data protected by the reader-writer lock.

#### `function release( self ) -> Void`

Release the read lock, decrementing the reader count and allowing a pending writer to acquire exclusive access if this was the last reader. After calling release, this guard must not be used again.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `WriteGuard`<T>

Exclusive write access token for an RwLock<T>. Only one WriteGuard can exist at a time, and no ReadGuards can coexist with it. The guard provides both read and write access to the protected data and must be explicitly released via release().

### Fields

| Name | Type | Access |
|------|------|--------|
| `lock` | `RwLock<T>` | protect |

### Methods

#### `function WriteGuard( self, RwLock<T> lock ) -> Void`

Create a new write guard bound to the given reader-writer lock.

**Parameters**:

- `lock` (`RwLock<T>`)
- `The reader-writer lock this guard provides access to.`

#### `function get( self ) -> T`

Read the protected data.

**Returns**: `T` — The current value of the data protected by the reader-writer lock.

#### `function set( self, T value ) -> Void`

Write a new value to the protected data.

**Parameters**:

- `value` (`T`)
- `The new value to store.`

#### `function release( self ) -> Void`

Release the write lock, allowing pending readers or writers to acquire access. After calling release, this guard must not be used again.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `RwLock`<T>

Data-protecting reader-writer lock. Allows multiple concurrent readers or a single exclusive writer. Owns the protected data -- access is only granted through ReadGuard or WriteGuard instances obtained from readLock()/writeLock() or their non-blocking tryReadLock()/tryWriteLock() variants.

Supports poison detection: if a thread panics while holding a write lock, subsequent lock attempts will raise a PoisonError. Built on ReadWriteLock from the OS sync module with cooperative blocking through the scheduler when contention occurs.

### Fields

| Name | Type | Access |
|------|------|--------|
| `inner` | `ReadWriteLock` | protect |
| `data` | `T` | protect |
| `scheduler` | `Scheduler` | protect |
| `poisoned` | `AtomicBoolean` | protect |

### Methods

#### `function RwLock( self, T value, Scheduler scheduler ) -> Void`

Create a new reader-writer lock protecting the given initial value.

**Parameters**:

- `value` (`T`)
- `The initial value to protect.`
- `scheduler` (`Scheduler`)
- `The scheduler to use for cooperative blocking when the lock`
- `is contended.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readLock( self ) -> ReadGuard<T>`

Acquire a shared read lock, blocking the current task until the lock is available. Multiple readers can hold the lock simultaneously, but a pending or active writer will block new readers.

**Returns**: `ReadGuard<T>` — A guard granting shared read access to the protected data.

**Raises**:

- `PoisonError` — If the lock was poisoned by a panicked writer.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function writeLock( self ) -> WriteGuard<T>`

Acquire an exclusive write lock, blocking the current task until no other readers or writers hold the lock.

**Returns**: `WriteGuard<T>` — A guard granting exclusive read-write access to the protected data.

**Raises**:

- `PoisonError` — If the lock was poisoned by a panicked writer.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function tryReadLock( self ) -> ?ReadGuard<T>`

Attempt to acquire a shared read lock without blocking. Returns immediately whether or not the lock was acquired.

**Returns**: — ?ReadGuard<T>:
A guard granting shared read access if acquired, or None
if the lock is held by a writer or is poisoned.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function tryWriteLock( self ) -> ?WriteGuard<T>`

Attempt to acquire an exclusive write lock without blocking. Returns immediately whether or not the lock was acquired.

**Returns**: — ?WriteGuard<T>:
A guard granting exclusive access if acquired, or None if the
lock is held by any reader or writer, or is poisoned.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getData( self ) -> T`

Return the raw protected data. Intended for internal use by guards only -- callers should access data through ReadGuard or WriteGuard.

**Returns**: `T` — The current value of the protected data.

#### `function setData( self, T value ) -> Void`

Replace the protected data. Intended for internal use by WriteGuard only -- callers should modify data through the guard.

**Parameters**:

- `value` (`T`)
- `The new value to store.`

#### `function readUnlockInternal( self ) -> Void`

Release a read lock and yield the current task. Called by ReadGuard.release().

#### `function writeUnlockInternal( self ) -> Void`

Release the write lock and yield the current task. Called by WriteGuard.release().

#### `function poison( self ) -> Void`

Mark the lock as poisoned. Subsequent lock attempts will raise a PoisonError. Typically called when a thread panics while holding the write lock.

#### `function isPoisoned( self ) -> Boolean`

Check whether the lock has been poisoned.

**Returns**: `Boolean` — True if the lock was poisoned by a panicked writer.

#### `function destroy( self ) -> Void`

Release resources held by the lock's poison flag.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

