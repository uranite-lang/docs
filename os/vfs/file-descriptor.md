# uranite.os.vfs.file-descriptor

## Table of Contents

- [Imports](#imports)
- [const `FLAG_READ`](#const-flag-read)
- [const `FLAG_WRITE`](#const-flag-write)
- [const `FLAG_APPEND`](#const-flag-append)
- [const `FLAG_CREATE`](#const-flag-create)
- [const `FLAG_TRUNCATE`](#const-flag-truncate)
- [class `FileDescriptor`](#class-filedescriptor)
  - [`FileDescriptor()`](#FileDescriptor)
  - [`canRead()`](#canRead)
  - [`canWrite()`](#canWrite)
  - [`isAppend()`](#isAppend)
  - [`seek()`](#seek)
  - [`advanceOffset()`](#advanceOffset)
  - [`retain()`](#retain)
  - [`release()`](#release)
  - [`getFdNumber()`](#getFdNumber)
  - [`getVnodeId()`](#getVnodeId)
  - [`getOffset()`](#getOffset)
- [class `FileDescriptorTable`](#class-filedescriptortable)
  - [`FileDescriptorTable()`](#FileDescriptorTable)
  - [`open()`](#open)
  - [`close()`](#close)
  - [`getOffset()`](#getOffset)
  - [`setOffset()`](#setOffset)
  - [`getVnodeId()`](#getVnodeId)
  - [`canRead()`](#canRead)
  - [`canWrite()`](#canWrite)
  - [`isOpen()`](#isOpen)
  - [`getOpenCount()`](#getOpenCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## const `FLAG_READ`

File access flag for read permission (bit 0). 

## const `FLAG_WRITE`

File access flag for write permission (bit 1). 

## const `FLAG_APPEND`

File access flag for append mode (bit 2). Writes go to end of file. 

## const `FLAG_CREATE`

File access flag for creation (bit 3). File created if not existing. 

## const `FLAG_TRUNCATE`

File access flag for truncation (bit 4). Contents discarded on open for write. 

## class `FileDescriptor`

Represents an open file descriptor, which is per-open-file state maintained by the kernel. The same VNode (file) can have multiple open file descriptors, each with its own independent file offset and access flags. Reference counting allows the descriptor to be shared across forked processes or duplicated file descriptors.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fdNumber` | `I64` | public |
| `vnodeId` | `I64` | public |
| `offset` | `I64` | public |
| `flags` | `I64` | public |
| `refCount` | `I64` | public |

### Methods

#### `function FileDescriptor( self, I64 fd, I64 vnode, I64 openFlags ) -> Void`

Constructs a new file descriptor with the given fd number, associated VNode identifier, and open flags. The file offset starts at zero and the reference count is initialized to 1.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function canRead( self ) -> Boolean`

Returns True if the read permission flag (bit 0) is set on this file descriptor. 

#### `function canWrite( self ) -> Boolean`

Returns True if the write permission flag (bit 1) is set on this file descriptor. 

#### `function isAppend( self ) -> Boolean`

Returns True if the append mode flag (bit 2) is set on this file descriptor. 

#### `function seek( self, I64 newOffset ) -> Void`

Sets the file offset to the specified position in bytes from the beginning of the file. 

#### `function advanceOffset( self, I64 bytesRead ) -> Void`

Advances the file offset by the specified number of bytes. This is called after a successful read or write operation to update the position for the next operation.

#### `function retain( self ) -> I64`

Increments the reference count by one and returns the new count. Called when the file descriptor is duplicated or inherited by a forked process.

#### `function release( self ) -> Boolean`

Decrements the reference count by one. Returns True if the reference count reached zero, indicating the file descriptor should be fully closed and its resources freed.

#### `function getFdNumber( self ) -> I64`

Returns the integer file descriptor number assigned to this open file. 

#### `function getVnodeId( self ) -> I64`

Returns the VNode identifier of the file associated with this descriptor. 

#### `function getOffset( self ) -> I64`

Returns the current file offset position in bytes.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `FileDescriptorTable`

Per-process table that maps file descriptor numbers to open file state (VNode ID, offset, flags). The table has a fixed capacity and uses free-list chaining for O(1) allocation and deallocation of file descriptor numbers. A sentinel value of -2 in the nextFree array marks an allocated (in-use) slot, while free slots chain to the next available slot.

### Fields

| Name | Type | Access |
|------|------|--------|
| `vnodeIds` | `Memory<I64>` | protect |
| `offsets` | `Memory<I64>` | protect |
| `fdFlags` | `Memory<I64>` | protect |
| `nextFree` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `freeHead` | `I64` | protect |
| `openCount` | `I64` | protect |

### Methods

#### `function FileDescriptorTable( self, I64 maxFds ) -> Void`

Constructs a new file descriptor table with the specified maximum number of descriptors. All slots are initialized as free and chained together into a free list, with the last slot marking the end of the list with -1.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function open( self, I64 vnodeId, I64 flags ) -> I64`

Allocates a file descriptor for the given VNode with the specified access flags. The lowest available fd number is assigned from the free list. The file offset starts at zero. Returns the fd number on success, or -1 if the table is full.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function close( self, I64 fd ) -> I64`

Closes the file descriptor at the given fd number, returning the slot to the free list. Returns the VNode ID that was referenced by the descriptor, so the caller can release the VNode reference. Returns -1 if the fd is out of bounds or not currently open.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getOffset( self, I64 fd ) -> I64`

Returns the current file offset for the given file descriptor number. Returns 0 if the fd is out of bounds.

#### `function setOffset( self, I64 fd, I64 offset ) -> Void`

Updates the file offset for the given file descriptor number. No action is taken if the fd is out of bounds.

#### `function getVnodeId( self, I64 fd ) -> I64`

Returns the VNode identifier associated with the given file descriptor number. Returns -1 if the fd is out of bounds.

#### `function canRead( self, I64 fd ) -> Boolean`

Checks whether the given file descriptor has read permission. Returns False if the fd is out of bounds or the read flag is not set.

#### `function canWrite( self, I64 fd ) -> Boolean`

Checks whether the given file descriptor has write permission. Returns False if the fd is out of bounds or the write flag is not set.

#### `function isOpen( self, I64 fd ) -> Boolean`

Returns True if the given file descriptor number is currently open (allocated and in use). Returns False if the fd is out of bounds or is in the free list.

#### `function getOpenCount( self ) -> I64`

Returns the number of currently open file descriptors in this table. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the file descriptor table, including the VNode ID, offset, flags, and free-list arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

