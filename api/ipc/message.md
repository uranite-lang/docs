# uranite.ipc.message

## Table of Contents

- [Imports](#imports)
- [class `MessageChannel`](#class-messagechannel)
  - [`MessageChannel()`](#MessageChannel)
  - [`send()`](#send)
  - [`sendMultiWord()`](#sendMultiWord)
  - [`receive()`](#receive)
  - [`peek()`](#peek)
  - [`pending()`](#pending)
  - [`isFull()`](#isFull)
  - [`isEmpty()`](#isEmpty)
  - [`getPortId()`](#getPortId)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.ipc.message`
  - `Message`
  - `Port`

## class `MessageChannel`

Bidirectional message-passing channel built on kernel Port primitives. Each channel owns a single port for queuing messages. Messages are fixed-size with word-based payloads serialized into ring buffer slots.

### Fields

| Name | Type | Access |
|------|------|--------|
| `port` | `Port` | protect |
| `wordsPerMessage` | `I64` | protect |

### Methods

#### `function MessageChannel( self, I64 portId, I64 ownerTaskId, I64 slotCount, I64 wordsPerMessage ) -> Void`

Construct a message channel with a kernel port.

**Parameters**:

- `portId` (`I64`)
- `Unique identifier for the underlying port.`
- `ownerTaskId` (`I64`)
- `Task identifier of the port owner.`
- `slotCount` (`I64`)
- `Number of message slots in the ring buffer.`
- `wordsPerMessage` (`I64`)
- `Maximum payload words per message` (`excluding header`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function send( self, I64 senderId, I64 messageType, I64 payload ) -> Boolean`

Send a single-word message through this channel.

**Parameters**:

- `senderId` (`I64`)
- `Identifier of the sending task.`
- `messageType` (`I64`)
- `Application-defined message type tag.`
- `payload` (`I64`)
- `Single word payload value.`

**Returns**: — Boolean:
True if message was enqueued, False if port is full.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function sendMultiWord( self, I64 senderId, I64 messageType, Message message ) -> Boolean`

Send a pre-built multi-word message through this channel. Caller is responsible for populating message fields.

**Parameters**:

- `senderId` (`I64`)
- `Identifier of the sending task.`
- `messageType` (`I64`)
- `Application-defined message type tag.`
- `message` (`Message`)
- `Pre-populated message with payload data.`

**Returns**: `Boolean` — True if message was enqueued, False if port is full.

#### `function receive( self, Message destination ) -> Boolean`

Receive the oldest message from this channel into destination.

**Parameters**:

- `destination` (`Message`)
- `Message object to populate with received data.`

**Returns**: `Boolean` — True if a message was dequeued, False if port is empty.

#### `function peek( self, Message destination ) -> Boolean`

Read the oldest message without removing it from the queue.

**Parameters**:

- `destination` (`Message`)
- `Message object to populate with peeked data.`

**Returns**: `Boolean` — True if a message was available, False if port is empty.

#### `function pending( self ) -> I64`

Return the number of messages currently queued. 

#### `function isFull( self ) -> Boolean`

Return whether the channel's ring buffer is full. 

#### `function isEmpty( self ) -> Boolean`

Return whether the channel has no queued messages. 

#### `function getPortId( self ) -> I64`

Return the underlying port identifier. 

#### `function destroy( self ) -> Void`

Free all resources held by this channel's port.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

