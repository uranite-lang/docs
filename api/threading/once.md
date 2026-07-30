# uranite.threading.once

## Table of Contents

- [Imports](#imports)
- [const `ONCE_NOT_CALLED`](#const-once-not-called)
- [const `ONCE_IN_PROGRESS`](#const-once-in-progress)
- [const `ONCE_COMPLETED`](#const-once-completed)
- [class `Once`](#class-once)
  - [`Once()`](#Once)
  - [`callOnce()`](#callOnce)
  - [`isCompleted()`](#isCompleted)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.sync.spinlock`
  - `Spinlock`
- `uranite.threading.atomic`
  - `AtomicI64`

## const `ONCE_NOT_CALLED`

State value indicating the initializer has not yet been called.

## const `ONCE_IN_PROGRESS`

State value indicating the initializer is currently executing.

## const `ONCE_COMPLETED`

State value indicating the initializer has finished execution.

## class `Once`

One-time initialization primitive that guarantees a callable runs exactly once, even when callOnce is invoked concurrently from multiple threads. The first thread to call callOnce executes the initializer while all other threads spin-wait until initialization is complete.

Uses a spinlock to protect the state transition from NotCalled to InProgress, and atomic state to allow lock-free fast-path checks once initialization is complete.

### Fields

| Name | Type | Access |
|------|------|--------|
| `state` | `AtomicI64` | protect |
| `guard` | `Spinlock` | protect |

### Methods

#### `function Once( self ) -> Void`

Create a new Once instance in the NotCalled state.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function callOnce( self, <type> initializer ) -> Void`

Execute the initializer function exactly once. If the initializer has already completed, this returns immediately. If another thread is currently running the initializer, this spins until completion. If no thread has started initialization, this thread acquires the lock, runs the initializer, and marks initialization as complete.

**Parameters**:

- `initializer` (`Callable<Void, <>>`)
- `The zero-argument function to call exactly once.`

**Complexity**:
- Time: `O(1) when already completed`
- Space: `O(1)`

#### `function callOnceRaw( self, I64 initializerAddr ) -> Void`

Execute the initializer from a raw function address. Internal use.

**Parameters**:

- `initializerAddr` (`I64`)
- `Raw address of the zero-argument function to call.`

**Complexity**:
- Time: `O(1) when already completed`
- Space: `O(1)`

#### `function isCompleted( self ) -> Boolean`

Check whether the initializer has finished execution.

**Returns**: `Boolean` — True if callOnce has successfully completed, False otherwise.

#### `function destroy( self ) -> Void`

Release resources held by the atomic state variable.

