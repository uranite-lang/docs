# uranite.os.io.request

## Table of Contents

- [Imports](#imports)
- [enum `RequestType`](#enum-requesttype)
- [enum `RequestStatus`](#enum-requeststatus)
- [class `IoRequestQueue`](#class-iorequestqueue)
  - [`IoRequestQueue()`](#IoRequestQueue)
  - [`submit()`](#submit)
  - [`dequeue()`](#dequeue)
  - [`complete()`](#complete)
  - [`fail()`](#fail)
  - [`drain()`](#drain)
  - [`getStatus()`](#getStatus)
  - [`getTag()`](#getTag)
  - [`getRequestType()`](#getRequestType)
  - [`getPending()`](#getPending)
  - [`getCompletedCount()`](#getCompletedCount)
  - [`isFull()`](#isFull)
  - [`isEmpty()`](#isEmpty)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## enum `RequestType`

Enumerates the types of I/O requests that can be submitted to the block device layer. 

## enum `RequestStatus`

Enumerates the lifecycle states of an I/O request in the queue. 

## class `IoRequestQueue`

Fixed-size ring buffer of asynchronous I/O requests submitted by the VFS or device drivers and processed by the block device layer. Each request tracks its type, target device, block number, buffer address, transfer length, status, and a caller-provided correlation tag.

### Fields

| Name | Type | Access |
|------|------|--------|
| `requestTypes` | `Memory<I64>` | protect |
| `deviceIds` | `Memory<I64>` | protect |
| `blockNumbers` | `Memory<I64>` | protect |
| `bufferAddresses` | `Memory<I64>` | protect |
| `lengths` | `Memory<I64>` | protect |
| `statuses` | `Memory<I64>` | protect |
| `tags` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `head` | `I64` | protect |
| `tail` | `I64` | protect |
| `pending` | `I64` | protect |
| `completedCount` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function IoRequestQueue( self, I64 queueSize ) -> Void`

Construct a new I/O request queue with the given maximum capacity. All request slots are initialized to zero.

**Parameters**:

- `queueSize` (`I64`)
- `The maximum number of concurrent I/O requests the queue can hold.`

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function submit( self, I64 reqType, I64 deviceId, I64 blockNumber, I64 bufferAddr, I64 length, I64 tag ) -> I64`

Submit an I/O request to the queue. The request is placed at the tail of the ring buffer with an initial status of pending.

**Parameters**:

- `reqType` (`I64`)
- `The request type` (`use requestRead, requestWrite, or requestFlush`)
- `deviceId` (`I64`)
- `The identifier of the target block device.`
- `blockNumber` (`I64`)
- `The starting block number for the operation.`
- `bufferAddr` (`I64`)
- `The memory address of the data buffer.`
- `length` (`I64`)
- `The number of bytes to transfer.`
- `tag` (`I64`)
- `A caller-provided correlation identifier for tracking the request.`

**Returns**: — I64:
The request index on success, or -1 if the queue is full.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function dequeue( self ) -> I64`

Get the next pending request index at the head of the queue for processing. Transitions the request status from pending to active.

**Returns**: — I64:
The request index of the next pending request, or -1 if none are available.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function complete( self, I64 index ) -> Boolean`

Mark the request at the given index as completed and increment the completed request counter.

**Parameters**:

- `index` (`I64`)
- `The request index to mark as completed.`

**Returns**: — Boolean:
True if the request was successfully marked, False if the index
is out of bounds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fail( self, I64 index ) -> Boolean`

Mark the request at the given index as failed.

**Parameters**:

- `index` (`I64`)
- `The request index to mark as failed.`

**Returns**: — Boolean:
True if the request was successfully marked, False if the index
is out of bounds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function drain( self ) -> I64`

Advance the head pointer past all completed or failed requests at the front of the ring buffer, freeing those slots for reuse. Stops at the first request that is still pending or active.

**Returns**: — I64:
The number of requests that were drained.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getStatus( self, I64 index ) -> I64`

Return the current status of the request at the given index.

**Parameters**:

- `index` (`I64`)
- `The request index to query.`

**Returns**: `I64` — The status value (0=pending, 1=active, 2=completed, 3=failed), or -1 if the index is out of bounds.

#### `function getTag( self, I64 index ) -> I64`

Return the caller-provided correlation tag for the request at the given index.

**Parameters**:

- `index` (`I64`)
- `The request index to query.`

**Returns**: `I64` — The tag value, or 0 if the index is out of bounds.

#### `function getRequestType( self, I64 index ) -> I64`

Return the request type for the request at the given index.

**Parameters**:

- `index` (`I64`)
- `The request index to query.`

**Returns**: `I64` — The request type value (1=read, 2=write, 3=flush), or 0 if the index is out of bounds.

#### `function getPending( self ) -> I64`

Return the number of pending requests currently in the queue. 

#### `function getCompletedCount( self ) -> I64`

Return the total number of requests that have been completed since queue creation. 

#### `function isFull( self ) -> Boolean`

Return True if the request queue is full and cannot accept new submissions. 

#### `function isEmpty( self ) -> Boolean`

Return True if there are no pending requests in the queue. 

#### `function destroy( self ) -> Void`

Free all memory buffers allocated by this I/O request queue.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

