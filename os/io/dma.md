# uranite.os.io.dma

## Table of Contents

- [Imports](#imports)
- [enum `DmaDirection`](#enum-dmadirection)
- [class `DmaDescriptor`](#class-dmadescriptor)
  - [`DmaDescriptor()`](#DmaDescriptor)
  - [`configure()`](#configure)
  - [`isComplete()`](#isComplete)
  - [`markComplete()`](#markComplete)
  - [`markError()`](#markError)
  - [`isError()`](#isError)
- [class `DmaChannel`](#class-dmachannel)
  - [`DmaChannel()`](#DmaChannel)
  - [`submit()`](#submit)
  - [`complete()`](#complete)
  - [`isComplete()`](#isComplete)
  - [`drain()`](#drain)
  - [`getPending()`](#getPending)
  - [`getChannelId()`](#getChannelId)
  - [`isFull()`](#isFull)
  - [`isEmpty()`](#isEmpty)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## enum `DmaDirection`

Enumerates the transfer directions for DMA operations between RAM and hardware devices. 

## class `DmaDescriptor`

Represents a single DMA transfer unit describing the source address, destination address, transfer length, direction, completion status, and a caller-provided tag for correlation. Status values: 0 = pending, 1 = complete, 2 = error.

### Fields

| Name | Type | Access |
|------|------|--------|
| `sourceAddress` | `I64` | public |
| `destAddress` | `I64` | public |
| `length` | `I64` | public |
| `direction` | `I64` | public |
| `status` | `I64` | public |
| `tag` | `I64` | public |

### Methods

#### `function DmaDescriptor( self ) -> Void`

Construct a new DMA descriptor with all fields initialized to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function configure( self, I64 src, I64 dst, I64 len, I64 dir ) -> Void`

Configure this DMA descriptor for a transfer operation, resetting the status to pending.

**Parameters**:

- `src` (`I64`)
- `The physical source address for the transfer.`
- `dst` (`I64`)
- `The physical destination address for the transfer.`
- `len` (`I64`)
- `The number of bytes to transfer.`
- `dir` (`I64`)
- `The transfer direction flag` (`use dmaToDevice, dmaFromDevice, or dmaBidirectional`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isComplete( self ) -> Boolean`

Return True if this DMA transfer has completed successfully (status equals 1). 

#### `function markComplete( self ) -> Void`

Mark this DMA descriptor as successfully completed by setting status to 1. 

#### `function markError( self ) -> Void`

Mark this DMA descriptor as failed by setting status to 2. 

#### `function isError( self ) -> Boolean`

Return True if this DMA transfer encountered an error (status equals 2).

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `DmaChannel`

Manages a ring buffer of DMA transfer descriptors for a single hardware DMA channel. Transfers are submitted to the tail and completed descriptors are drained from the head. The channel tracks pending transfer count and protects concurrent access with a spinlock.

### Fields

| Name | Type | Access |
|------|------|--------|
| `sources` | `Memory<I64>` | protect |
| `destinations` | `Memory<I64>` | protect |
| `lengths` | `Memory<I64>` | protect |
| `directions` | `Memory<I64>` | protect |
| `statuses` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `head` | `I64` | protect |
| `tail` | `I64` | protect |
| `pending` | `I64` | protect |
| `channelId` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function DmaChannel( self, I64 channelId, I64 ringSize ) -> Void`

Construct a new DMA channel with the given channel identifier and ring buffer capacity. All descriptor slots are initialized to zero.

**Parameters**:

- `channelId` (`I64`)
- `The hardware DMA channel identifier.`
- `ringSize` (`I64`)
- `The maximum number of concurrent DMA descriptors in the ring buffer.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function submit( self, I64 src, I64 dst, I64 len, I64 dir ) -> I64`

Submit a DMA transfer to this channel by writing source, destination, length, and direction into the next available ring slot. The operation is protected by a spinlock.

**Parameters**:

- `src` (`I64`)
- `The physical source address for the transfer.`
- `dst` (`I64`)
- `The physical destination address for the transfer.`
- `len` (`I64`)
- `The number of bytes to transfer.`
- `dir` (`I64`)
- `The transfer direction flag.`

**Returns**: — I64:
The descriptor index on success, or -1 if the ring is full.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function complete( self, I64 index ) -> Boolean`

Mark the descriptor at the given index as completed. This is typically called from a DMA interrupt handler when the hardware signals transfer completion.

**Parameters**:

- `index` (`I64`)
- `The descriptor index to mark as complete.`

**Returns**: — Boolean:
True if the descriptor was successfully marked, False if the index
is out of bounds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isComplete( self, I64 index ) -> Boolean`

Check whether the descriptor at the given index has completed its transfer.

**Parameters**:

- `index` (`I64`)
- `The descriptor index to check.`

**Returns**: `Boolean` — True if the descriptor status indicates completion.

#### `function drain( self ) -> I64`

Advance the head pointer past all completed descriptors at the front of the ring buffer. Stops at the first non-completed descriptor.

**Returns**: — I64:
The number of descriptors that were drained.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getPending( self ) -> I64`

Return the number of pending (in-flight) DMA transfers on this channel. 

#### `function getChannelId( self ) -> I64`

Return the hardware DMA channel identifier. 

#### `function isFull( self ) -> Boolean`

Return True if the DMA ring buffer is full and cannot accept new transfers. 

#### `function isEmpty( self ) -> Boolean`

Return True if there are no pending DMA transfers on this channel. 

#### `function destroy( self ) -> Void`

Free all memory buffers allocated by this DMA channel.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

