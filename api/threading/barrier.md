# uranite.threading.barrier

## Table of Contents

- [Imports](#imports)
- [class `Barrier`](#class-barrier)
  - [`Barrier()`](#Barrier)
  - [`wait()`](#wait)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.ipc.signal`
  - `Event`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.sync.spinlock`
  - `Spinlock`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.threading.atomic`
  - `AtomicI64`

## class `Barrier`

N-thread synchronization barrier that blocks all participating threads until the last one arrives. Uses a generation counter to support reuse across multiple barrier cycles. The last thread to arrive resets the counter and signals all waiters. Exactly one thread per cycle receives True from wait() (the "leader"), all others receive False.

### Fields

| Name | Type | Access |
|------|------|--------|
| `threshold` | `I64` | protect |
| `arrived` | `AtomicI64` | protect |
| `generation` | `AtomicI64` | protect |
| `guard` | `Spinlock` | protect |
| `event` | `Event` | protect |
| `scheduler` | `Scheduler` | protect |

### Methods

#### `function Barrier( self, I64 count, Scheduler scheduler ) -> Void`

Construct a barrier that requires the specified number of threads to arrive before any can proceed.

**Parameters**:

- `count` (`I64`)
- `The number of threads that must call wait`
- `barrier opens.`
- `scheduler` (`Scheduler`)
- `The scheduler used for cooperative task blocking.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function wait( self ) -> Boolean`

Block the calling thread until all threads have arrived at the barrier. The last thread to arrive opens the barrier and wakes all waiters.

**Returns**: — Boolean:
True for exactly one thread per cycle (the "leader" that opened
the barrier), False for all other threads.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release atomic resources used by the barrier. The barrier must not be used after this call.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

