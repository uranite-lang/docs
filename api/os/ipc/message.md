# uranite.os.ipc.message

## Table of Contents

- [Imports](#imports)
- [class `Message`](#class-message)
  - [`Message()`](#Message)
  - [`setWord()`](#setWord)
  - [`getWord()`](#getWord)
  - [`getCapacity()`](#getCapacity)
  - [`getLength()`](#getLength)
  - [`clear()`](#clear)
  - [`copyFrom()`](#copyFrom)
  - [`destroy()`](#destroy)
- [class `Port`](#class-port)
  - [`Port()`](#Port)
  - [`isFull()`](#isFull)
  - [`isEmpty()`](#isEmpty)
  - [`getCount()`](#getCount)
  - [`getPortId()`](#getPortId)
  - [`getOwnerTaskId()`](#getOwnerTaskId)
  - [`send()`](#send)
  - [`receive()`](#receive)
  - [`peek()`](#peek)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `Message`

Fixed-size kernel IPC message consisting of a header (sender identifier, message type, payload length) and a data payload stored as an array of word-sized values. The maximum payload capacity is set at construction time.

### Fields

| Name | Type | Access |
|------|------|--------|
| `senderId` | `I64` | public |
| `messageType` | `I64` | public |
| `length` | `I64` | public |
| `data` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |

### Methods

#### `function Message( self, I64 maxWords ) -> Void`

Construct a new message with the given maximum payload capacity in words.

**Parameters**:

- `maxWords` (`I64`)
- `The maximum number of word-sized values this message can carry.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setWord( self, I64 index, I64 value ) -> Boolean`

Set a word in the message payload at the given index. If the index extends beyond the current length, the length is automatically expanded.

**Parameters**:

- `index` (`I64`)
- `The zero-based word index within the payload.`
- `value` (`I64`)
- `The word value to store.`

**Returns**: — Boolean:
True on success, False if the index is out of the message capacity.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getWord( self, I64 index ) -> I64`

Retrieve a word from the message payload at the given index.

**Parameters**:

- `index` (`I64`)
- `The zero-based word index within the payload.`

**Returns**: `I64` — The word value at the given index, or 0 if the index is out of range.

#### `function getCapacity( self ) -> I64`

Return the maximum number of words this message can hold. 

#### `function getLength( self ) -> I64`

Return the current number of words stored in this message payload. 

#### `function clear( self ) -> Void`

Reset the message header fields to zero without freeing the underlying data buffer. 

#### `function copyFrom( self, Message source ) -> Boolean`

Copy the entire payload and header from a source message into this message.

**Parameters**:

- `source` (`Message`)
- `The source message to copy data from.`

**Returns**: — Boolean:
True if the copy succeeded, False if the source payload exceeds
this message's capacity.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Free the underlying data buffer allocated by this message.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Port`

Kernel port object for IPC message delivery. Uses a circular ring buffer of fixed-size message slots where each slot contains a serialized message header (senderId, messageType, length) followed by the payload words. Access to send and receive operations is controlled by the capability system, and concurrent access is protected by a spinlock.

### Fields

| Name | Type | Access |
|------|------|--------|
| `portId` | `I64` | protect |
| `buffer` | `Memory<I64>` | protect |
| `slotSize` | `I64` | protect |
| `slotCount` | `I64` | protect |
| `head` | `I64` | protect |
| `tail` | `I64` | protect |
| `count` | `I64` | protect |
| `ownerTaskId` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function Port( self, I64 id, I64 owner, I64 slots, I64 wordsPerSlot ) -> Void`

Construct a new port with the given identifier, owner task, slot count, and words per message slot. Each slot stores a 3-word header (senderId, messageType, length) followed by the payload words.

**Parameters**:

- `id` (`I64`)
- `The unique port identifier.`
- `owner` (`I64`)
- `The task identifier of the port owner.`
- `slots` (`I64`)
- `The number of message slots in the ring buffer.`
- `wordsPerSlot` (`I64`)
- `The maximum number of payload words per message slot` (`excluding header`)

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function isFull( self ) -> Boolean`

Return True if the port ring buffer is full and cannot accept new messages. 

#### `function isEmpty( self ) -> Boolean`

Return True if the port ring buffer contains no messages. 

#### `function getCount( self ) -> I64`

Return the number of messages currently queued in this port. 

#### `function getPortId( self ) -> I64`

Return the unique identifier of this port. 

#### `function getOwnerTaskId( self ) -> I64`

Return the task identifier of the task that owns this port. 

#### `function send( self, Message msg ) -> Boolean`

Enqueue a message into the port ring buffer by serializing its header and payload into the next available slot. Protected by a spinlock.

**Parameters**:

- `msg` (`Message`)
- `The message to enqueue.`

**Returns**: — Boolean:
True if the message was successfully enqueued, False if the port is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function receive( self, Message dest ) -> Boolean`

Dequeue the oldest message from the port ring buffer by deserializing its header and payload into the destination message object. The slot is freed for reuse. Protected by a spinlock.

**Parameters**:

- `dest` (`Message`)
- `The destination message object to populate with the dequeued data.`

**Returns**: — Boolean:
True if a message was successfully dequeued, False if the port is empty.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function peek( self, Message dest ) -> Boolean`

Read the oldest message from the port ring buffer without removing it. The message remains in the queue for a subsequent receive call.

**Parameters**:

- `dest` (`Message`)
- `The destination message object to populate with the peeked data.`

**Returns**: — Boolean:
True if a message was available to peek, False if the port is empty.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Free the ring buffer memory allocated by this port.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

