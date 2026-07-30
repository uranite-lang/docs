# uranite.threading.channel

## Table of Contents

- [Imports](#imports)
- [class `ChannelInner`](#class-channelinner)
  - [`ChannelInner()`](#ChannelInner)
  - [`send()`](#send)
  - [`receive()`](#receive)
  - [`trySend()`](#trySend)
  - [`tryReceive()`](#tryReceive)
  - [`isClosed()`](#isClosed)
  - [`isEmpty()`](#isEmpty)
  - [`getCount()`](#getCount)
  - [`closeSend()`](#closeSend)
  - [`closeReceive()`](#closeReceive)
  - [`destroy()`](#destroy)
- [class `Sender`](#class-sender)
  - [`Sender()`](#Sender)
  - [`send()`](#send)
  - [`trySend()`](#trySend)
  - [`close()`](#close)
- [class `Receiver`](#class-receiver)
  - [`Receiver()`](#Receiver)
  - [`receive()`](#receive)
  - [`tryReceive()`](#tryReceive)
  - [`close()`](#close)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.scheduler.scheduler`
  - `Scheduler`
- `uranite.os.sync.semaphore`
  - `Semaphore`
- `uranite.os.sync.spinlock`
  - `Spinlock`
- `uranite.os.task.task`
  - `BlockReason`
- `uranite.threading.atomic`
  - `AtomicBoolean`
  - `AtomicI64`
- `uranite.threading.errors`
  - `ThreadChannelError`

## class `ChannelInner`<T>

Bounded circular buffer channel that forms the shared backbone of Sender/Receiver pairs. Supports multiple producers and a single consumer. Values are copied into the internal buffer -- no shared references cross thread boundaries. Blocking send and receive use semaphores with cooperative scheduler integration; non-blocking variants are also provided.

### Fields

| Name | Type | Access |
|------|------|--------|
| `buffer` | `Memory<T>` | protect |
| `capacity` | `I64` | protect |
| `head` | `I64` | protect |
| `tail` | `I64` | protect |
| `count` | `I64` | protect |
| `guard` | `Spinlock` | protect |
| `emptySlots` | `Semaphore` | protect |
| `fullSlots` | `Semaphore` | protect |
| `scheduler` | `Scheduler` | protect |
| `closed` | `AtomicBoolean` | protect |
| `senderCount` | `AtomicI64` | protect |

### Methods

#### `function ChannelInner( self, I64 capacity, Scheduler scheduler ) -> Void`

Construct a bounded channel with the specified capacity.

**Parameters**:

- `capacity` (`I64`)
- `Maximum number of values the channel can buffer.`
- `scheduler` (`Scheduler`)
- `Scheduler used for cooperative task blocking.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function send( self, T value ) -> Boolean`

Send a value into the channel, blocking if the buffer is full. Returns False immediately if the channel is closed.

**Parameters**:

- `value` (`T`)
- `The value to send.`

**Returns**: — Boolean:
True if the value was sent successfully, False if the channel
was closed before the send could complete.

**Complexity**:
- Time: `O(1) amortized, unbounded under contention`
- Space: `O(1)`

#### `function receive( self ) -> T`

Receive a value from the channel, blocking if the buffer is empty. Raises if the channel is closed and no values remain.

**Returns**: `T` — The next value from the channel.

**Raises**:

- `ThreadChannelError` → `ThreadError` → `Error` — If the channel is closed and empty.

**Complexity**:
- Time: `O(1) amortized, unbounded under contention`
- Space: `O(1)`

#### `function trySend( self, T value ) -> Boolean`

Attempt to send a value without blocking. Returns False immediately if the channel is closed or the buffer is full.

**Parameters**:

- `value` (`T`)
- `The value to send.`

**Returns**: — Boolean:
True if the value was sent, False if the channel is closed
or full.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function tryReceive( self ) -> ?T`

Attempt to receive a value without blocking. Returns None if no values are currently available.

**Returns**: — ?T:
The next value from the channel, or None if the buffer is empty.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isClosed( self ) -> Boolean`

Check whether the channel has been closed.

**Returns**: `Boolean` — True if the channel is closed, False otherwise.

#### `function isEmpty( self ) -> Boolean`

Check whether the channel buffer currently holds zero values.

**Returns**: `Boolean` — True if the buffer is empty, False otherwise.

#### `function getCount( self ) -> I64`

Return the number of values currently buffered in the channel.

**Returns**: `I64` — The current item count.

#### `function closeSend( self ) -> Void`

Decrement the sender reference count. When the last sender closes, the channel is marked as closed, preventing further sends.

#### `function closeReceive( self ) -> Void`

Close the channel from the receiver side. Immediately marks the channel as closed, which will cause pending and future sends to fail.

#### `function destroy( self ) -> Void`

Release all resources owned by the channel. Frees the internal buffer and destroys atomic state. The channel must not be used after this call.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Sender`<T>

Send half of a bounded channel. Wraps a ChannelInner and provides a simplified API for producers. Multiple Senders may share the same underlying ChannelInner for multi-producer patterns.

### Fields

| Name | Type | Access |
|------|------|--------|
| `inner` | `ChannelInner<T>` | protect |

### Methods

#### `function Sender( self, ChannelInner<T> inner ) -> Void`

Construct a sender connected to the given channel.

**Parameters**:

- `inner` (`ChannelInner<T>`)
- `The underlying channel to send values into.`

#### `function send( self, T value ) -> Void`

Send a value into the channel, blocking if the buffer is full. Raises if the channel has been closed.

**Parameters**:

- `value` (`T`)
- `The value to send.`

**Raises**:

- `ThreadChannelError` → `ThreadError` → `Error` — If the channel is closed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function trySend( self, T value ) -> Boolean`

Attempt to send a value without blocking.

**Parameters**:

- `value` (`T`)
- `The value to send.`

**Returns**: `Boolean` — True if the value was sent, False if the channel is closed or full.

#### `function close( self ) -> Void`

Close this sender's connection to the channel. If this is the last active sender, the channel itself is marked as closed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Receiver`<T>

Receive half of a bounded channel. Wraps a ChannelInner and provides a simplified API for the consumer. Only one Receiver should be active per channel (single-consumer pattern).

### Fields

| Name | Type | Access |
|------|------|--------|
| `inner` | `ChannelInner<T>` | protect |

### Methods

#### `function Receiver( self, ChannelInner<T> inner ) -> Void`

Construct a receiver connected to the given channel.

**Parameters**:

- `inner` (`ChannelInner<T>`)
- `The underlying channel to receive values from.`

#### `function receive( self ) -> T`

Receive a value from the channel, blocking if the buffer is empty.

**Returns**: `T` — The next value from the channel.

**Raises**:

- `ThreadChannelError` → `ThreadError` → `Error` — If the channel is closed and empty.

#### `function tryReceive( self ) -> ?T`

Attempt to receive a value without blocking.

**Returns**: `?T` — The next value, or None if the buffer is empty.

#### `function close( self ) -> Void`

Close the channel from the receiver side. All pending and future sends will fail after this call.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

