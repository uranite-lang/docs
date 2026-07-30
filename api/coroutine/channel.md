# uranite.coroutine.channel

## Table of Contents

- [Imports](#imports)
- [class `ChannelError`](#class-channelerror)
  - [`ChannelError()`](#ChannelError)
- [interface `AsyncChannel`](#interface-asyncchannel)
  - [`send()`](#send)
  - [`receive()`](#receive)
  - [`close()`](#close)
  - [`closed()`](#closed)
- [class `BufferedChannel`](#class-bufferedchannel)
  - [`BufferedChannel()`](#BufferedChannel)
  - [`send()`](#send)
  - [`receive()`](#receive)
  - [`close()`](#close)
  - [`closed()`](#closed)
  - [`size()`](#size)
  - [`empty()`](#empty)
  - [`destroy()`](#destroy)

## Imports

- `uranite.coroutine.scheduler`
  - `CoroutineScheduler`
- `uranite.errors.error`
  - `Error`
- `uranite.memory.memory`
  - `Memory`

## class `ChannelError`

**Extends**: `Error`

### Methods

#### `function ChannelError( self, String message ) -> Void`

## interface `AsyncChannel`<T>

### Methods

#### `function send( self, T value ) -> Void`

Send value into channel. Blocks if buffer full. 

#### `function receive( self ) -> T`

Receive value from channel. Blocks if empty. 

#### `function close( self ) -> Void`

Close channel. 

#### `function closed( self ) -> Boolean`

Check if channel is closed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `BufferedChannel`

**Implements**: `AsyncChannel<I64>`

### Fields

| Name | Type | Access |
|------|------|--------|
| `buffer` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `head` | `I64` | public |
| `tail` | `I64` | public |
| `count` | `I64` | public |
| `isClosed` | `Boolean` | public |
| `scheduler` | `CoroutineScheduler` | public |

### Methods

#### `function BufferedChannel( self, I64 capacity, CoroutineScheduler scheduler ) -> Void`

#### `function send( self, I64 value ) -> Void`

#### `function receive( self ) -> I64`

#### `function close( self ) -> Void`

#### `function closed( self ) -> Boolean`

#### `function size( self ) -> I64`

#### `function empty( self ) -> Boolean`

#### `function destroy( self ) -> Void`

