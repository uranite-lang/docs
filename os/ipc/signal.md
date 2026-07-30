# uranite.os.ipc.signal

## Table of Contents

- [Imports](#imports)
- [class `Event`](#class-event)
  - [`Event()`](#Event)
  - [`signal()`](#signal)
  - [`poll()`](#poll)
  - [`acknowledge()`](#acknowledge)
  - [`registerWait()`](#registerWait)
  - [`clearWait()`](#clearWait)
  - [`getWaitingTaskId()`](#getWaitingTaskId)
  - [`getSignaled()`](#getSignaled)
  - [`reset()`](#reset)

## Imports

- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `Event`

Bitmask-based notification primitive for kernel inter-process communication. Each bit in the signaled bitmask represents an independent event channel. Tasks can signal specific bits, poll for signaled bits without blocking, acknowledge and consume signaled bits, or register a wait for the scheduler to check when the task should be unblocked.

### Fields

| Name | Type | Access |
|------|------|--------|
| `signaled` | `I64` | protect |
| `waitingTaskId` | `I64` | protect |
| `waitMask` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function Event( self ) -> Void`

Construct a new event object with no bits signaled and no registered waiter.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function signal( self, I64 bits ) -> Boolean`

Signal specific event bits by setting them in the signaled bitmask. If a waiter is registered and the signaled bits satisfy the wait mask, returns True to indicate the waiter should be unblocked.

**Parameters**:

- `bits` (`I64`)
- `The bitmask of event bits to signal.`

**Returns**: — Boolean:
True if a registered waiter was satisfied by the newly signaled bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function poll( self, I64 mask ) -> I64`

Check which bits matching the given mask are currently signaled without blocking or consuming them.

**Parameters**:

- `mask` (`I64`)
- `The bitmask of event bits to check.`

**Returns**: `I64` — The subset of bits from the mask that are currently signaled.

#### `function acknowledge( self, I64 mask ) -> I64`

Consume signaled bits matching the given mask by clearing them from the signaled bitmask and returning which bits were set. This is an atomic test-and-clear operation protected by a spinlock.

**Parameters**:

- `mask` (`I64`)
- `The bitmask of event bits to acknowledge and consume.`

**Returns**: — I64:
The subset of bits from the mask that were signaled before clearing.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function registerWait( self, I64 taskId, I64 mask ) -> Boolean`

Register a task to wait for specific event bits. Only one waiter can be registered per event object. The scheduler checks whether the wait condition is satisfied to decide when to unblock the task. If the requested bits are already signaled at registration time, returns False immediately (no blocking needed).

**Parameters**:

- `taskId` (`I64`)
- `The identifier of the task that wants to wait.`
- `mask` (`I64`)
- `The bitmask of event bits the task is waiting for.`

**Returns**: — Boolean:
True if the requested bits are already signaled (no blocking needed),
False if the wait was registered and the task should block or another
waiter is already registered.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function clearWait( self ) -> Void`

Clear the wait registration, called when the waiting task is unblocked by the scheduler. 

#### `function getWaitingTaskId( self ) -> I64`

Return the task identifier of the currently registered waiter, or 0 if no task is waiting. 

#### `function getSignaled( self ) -> I64`

Return the current signaled bitmask. 

#### `function reset( self ) -> Void`

Clear all signaled event bits, resetting the event to its initial state.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

