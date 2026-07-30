# uranite.os.sync.read-write-lock

## Table of Contents

- [Imports](#imports)
- [class `ReadWriteLock`](#class-readwritelock)
  - [`ReadWriteLock()`](#ReadWriteLock)
  - [`readLock()`](#readLock)
  - [`readUnlock()`](#readUnlock)
  - [`writeLock()`](#writeLock)
  - [`writeUnlock()`](#writeUnlock)
  - [`getReaderCount()`](#getReaderCount)
  - [`isWriterActive()`](#isWriterActive)

## Imports

- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `ReadWriteLock`

Read-write lock allowing multiple concurrent readers or one exclusive writer. Uses writer-preference policy: once a writer is waiting, new readers will queue behind it to prevent writer starvation. A spinlock protects all internal state transitions.

### Fields

| Name | Type | Access |
|------|------|--------|
| `guard` | `Spinlock` | protect |
| `readerCount` | `I64` | protect |
| `writerActive` | `Boolean` | protect |
| `writerWaiting` | `I64` | protect |

### Methods

#### `function ReadWriteLock( self ) -> Void`

Construct a read-write lock in the unlocked state with no active readers or writers and no waiting writers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readLock( self ) -> Boolean`

Attempt to acquire the read lock. Returns True if acquired successfully. Returns False if a writer is currently active or waiting, enforcing writer-preference to prevent writer starvation. The caller should block the current task via the scheduler when False is returned.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readUnlock( self ) -> Boolean`

Release the read lock and decrement the reader count. Returns True if the unlock succeeded (a read lock was held). Returns False if there were no active readers, meaning the caller attempted an unbalanced unlock.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeLock( self ) -> Boolean`

Attempt to acquire the exclusive write lock. Returns True if acquired successfully, meaning no readers are active and no other writer holds the lock. Returns False if the lock is currently held, in which case the writer waiting count is incremented and the caller should block.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeUnlock( self ) -> I64`

Release the exclusive write lock and return the number of writers still waiting. The caller can use this to decide whether to wake another writer or allow queued readers to proceed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getReaderCount( self ) -> I64`

Return the number of readers currently holding the read lock. 

#### `function isWriterActive( self ) -> Boolean`

Return whether a writer currently holds the exclusive write lock. 

