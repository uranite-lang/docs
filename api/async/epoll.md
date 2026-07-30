# uranite.async.epoll

## Table of Contents

- [Imports](#imports)
- [const `SYS_EPOLL_CREATE1`](#const-sys-epoll-create1)
- [const `SYS_EPOLL_CTL`](#const-sys-epoll-ctl)
- [const `SYS_EPOLL_WAIT`](#const-sys-epoll-wait)
- [const `EPOLL_CTL_ADD`](#const-epoll-ctl-add)
- [const `EPOLL_CTL_DEL`](#const-epoll-ctl-del)
- [const `EPOLL_CTL_MOD`](#const-epoll-ctl-mod)
- [const `EPOLLIN`](#const-epollin)
- [const `EPOLLOUT`](#const-epollout)
- [const `EPOLLERR`](#const-epollerr)
- [const `EPOLLHUP`](#const-epollhup)
- [const `EPOLLRDHUP`](#const-epollrdhup)
- [const `EPOLLET`](#const-epollet)
- [const `EPOLLONESHOT`](#const-epolloneshot)
- [const `EPOLL_EVENT_SIZE`](#const-epoll-event-size)
- [function `epollCreate`](#function-epollcreate)
  - [`epollCreate()`](#epollCreate)
- [function `buildEpollEvent`](#function-buildepollevent)
- [function `epollAdd`](#function-epolladd)
  - [`epollAdd()`](#epollAdd)
- [function `epollDel`](#function-epolldel)
  - [`epollDel()`](#epollDel)
- [function `epollMod`](#function-epollmod)
  - [`epollMod()`](#epollMod)
- [function `epollWait`](#function-epollwait)
  - [`epollWait()`](#epollWait)
- [function `epollEventGetEvents`](#function-epolleventgetevents)
  - [`epollEventGetEvents()`](#epollEventGetEvents)
- [function `epollEventGetFd`](#function-epolleventgetfd)
  - [`epollEventGetFd()`](#epollEventGetFd)
- [function `allocateEventBuffer`](#function-allocateeventbuffer)
  - [`allocateEventBuffer()`](#allocateEventBuffer)

## Imports

- `uranite.async.errors`
  - `AsyncRuntimeError`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readI32At`
  - `readI64At`
  - `writeI32At`
  - `writeI64At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall1`
  - `syscall2`
  - `syscall4`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `SYS_EPOLL_CREATE1`

Syscall number for epoll_create1 on x86-64 Linux. 

## const `SYS_EPOLL_CTL`

Syscall number for epoll_ctl on x86-64 Linux. 

## const `SYS_EPOLL_WAIT`

Syscall number for epoll_wait on x86-64 Linux. 

## const `EPOLL_CTL_ADD`

epoll_ctl operation to register a new file descriptor. 

## const `EPOLL_CTL_DEL`

epoll_ctl operation to remove a registered file descriptor. 

## const `EPOLL_CTL_MOD`

epoll_ctl operation to modify events for a registered file descriptor. 

## const `EPOLLIN`

Event flag indicating the file descriptor is ready for reading. 

## const `EPOLLOUT`

Event flag indicating the file descriptor is ready for writing. 

## const `EPOLLERR`

Event flag indicating an error condition on the file descriptor. 

## const `EPOLLHUP`

Event flag indicating the peer closed its end of the connection. 

## const `EPOLLRDHUP`

Event flag indicating the peer shut down the writing half of the connection. 

## const `EPOLLET`

Event flag enabling edge-triggered notification mode. 

## const `EPOLLONESHOT`

Event flag disabling the descriptor after one event is reported. 

## const `EPOLL_EVENT_SIZE`

Size in bytes of a single epoll_event structure (4-byte events + 8-byte data). 

## function `epollCreate`

Create a new epoll instance via the epoll_create1 syscall.

**Returns**: `I64` — The file descriptor for the new epoll instance.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_create1 syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function epollCreate(  ) -> I64`

Create a new epoll instance via the epoll_create1 syscall.

**Returns**: `I64` — The file descriptor for the new epoll instance.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_create1 syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `buildEpollEvent`

### Methods

#### `function buildEpollEvent( I64 events, I64 fd ) -> Memory<I64>`

## function `epollAdd`

Register a file descriptor with an epoll instance for the specified events.

**Parameters**:

- `epfd` (`I64`)
- `The epoll instance file descriptor.`
- `fd` (`I64`)
- `The file descriptor to monitor.`
- `events` (`I64`)
- `Bitmask of event flags` (`EPOLLIN, EPOLLOUT, etc.`)

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_ctl ADD operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function epollAdd( I64 epfd, I64 fd, I64 events ) -> Void`

Register a file descriptor with an epoll instance for the specified events.

**Parameters**:

- `epfd` (`I64`)
- `The epoll instance file descriptor.`
- `fd` (`I64`)
- `The file descriptor to monitor.`
- `events` (`I64`)
- `Bitmask of event flags` (`EPOLLIN, EPOLLOUT, etc.`)

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_ctl ADD operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `epollDel`

Remove a file descriptor from an epoll instance.

**Parameters**:

- `epfd` (`I64`)
- `The epoll instance file descriptor.`
- `fd` (`I64`)
- `The file descriptor to stop monitoring.`

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_ctl DEL operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function epollDel( I64 epfd, I64 fd ) -> Void`

Remove a file descriptor from an epoll instance.

**Parameters**:

- `epfd` (`I64`)
- `The epoll instance file descriptor.`
- `fd` (`I64`)
- `The file descriptor to stop monitoring.`

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_ctl DEL operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `epollMod`

Modify the events monitored for a file descriptor in an epoll instance.

**Parameters**:

- `epfd` (`I64`)
- `The epoll instance file descriptor.`
- `fd` (`I64`)
- `The file descriptor whose monitored events should be changed.`
- `events` (`I64`)
- `The new bitmask of event flags to watch for.`

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_ctl MOD operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function epollMod( I64 epfd, I64 fd, I64 events ) -> Void`

Modify the events monitored for a file descriptor in an epoll instance.

**Parameters**:

- `epfd` (`I64`)
- `The epoll instance file descriptor.`
- `fd` (`I64`)
- `The file descriptor whose monitored events should be changed.`
- `events` (`I64`)
- `The new bitmask of event flags to watch for.`

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_ctl MOD operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `epollWait`

Wait for events on an epoll instance, blocking up to the specified timeout. Returns zero if interrupted by a signal (EINTR, errno 4) instead of raising.

**Parameters**:

- `epfd` (`I64`)
- `The epoll instance file descriptor.`
- `eventsBufPtr` (`I64`)
- `Pointer to the buffer that will receive epoll_event structures.`
- `maxEvents` (`I64`)
- `Maximum number of events to return in a single call.`
- `timeoutMs` (`I64`)
- `Timeout in milliseconds. Use -1 for indefinite blocking, 0 for`
- `non-blocking poll.`

**Returns**: `I64` — The number of ready file descriptors, or zero on signal interruption.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_wait syscall fails with an error other than EINTR.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function epollWait( I64 epfd, I64 eventsBufPtr, I64 maxEvents, I64 timeoutMs ) -> I64`

Wait for events on an epoll instance, blocking up to the specified timeout. Returns zero if interrupted by a signal (EINTR, errno 4) instead of raising.

**Parameters**:

- `epfd` (`I64`)
- `The epoll instance file descriptor.`
- `eventsBufPtr` (`I64`)
- `Pointer to the buffer that will receive epoll_event structures.`
- `maxEvents` (`I64`)
- `Maximum number of events to return in a single call.`
- `timeoutMs` (`I64`)
- `Timeout in milliseconds. Use -1 for indefinite blocking, 0 for`
- `non-blocking poll.`

**Returns**: `I64` — The number of ready file descriptors, or zero on signal interruption.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the epoll_wait syscall fails with an error other than EINTR.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `epollEventGetEvents`

Extract the event flags from an epoll_event at the given index in the event buffer.

**Parameters**:

- `bufPtr` (`I64`)
- `Pointer to the start of the epoll event buffer.`
- `index` (`I64`)
- `Zero-based index of the event to read.`

**Returns**: `I64` — The event flags bitmask for the specified event.

### Methods

#### `function epollEventGetEvents( I64 bufPtr, I64 index ) -> I64`

Extract the event flags from an epoll_event at the given index in the event buffer.

**Parameters**:

- `bufPtr` (`I64`)
- `Pointer to the start of the epoll event buffer.`
- `index` (`I64`)
- `Zero-based index of the event to read.`

**Returns**: `I64` — The event flags bitmask for the specified event.

## function `epollEventGetFd`

Extract the file descriptor from an epoll_event at the given index in the event buffer.

**Parameters**:

- `bufPtr` (`I64`)
- `Pointer to the start of the epoll event buffer.`
- `index` (`I64`)
- `Zero-based index of the event to read.`

**Returns**: `I64` — The file descriptor associated with the specified event.

### Methods

#### `function epollEventGetFd( I64 bufPtr, I64 index ) -> I64`

Extract the file descriptor from an epoll_event at the given index in the event buffer.

**Parameters**:

- `bufPtr` (`I64`)
- `Pointer to the start of the epoll event buffer.`
- `index` (`I64`)
- `Zero-based index of the event to read.`

**Returns**: `I64` — The file descriptor associated with the specified event.

## function `allocateEventBuffer`

Allocate a memory buffer large enough to hold the specified number of epoll_event structures. Each event occupies EPOLL_EVENT_SIZE bytes.

**Parameters**:

- `maxEvents` (`I64`)
- `The maximum number of events the buffer should accommodate.`

**Returns**: — Memory<I64>:
A newly allocated buffer sized for the requested event count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function allocateEventBuffer( I64 maxEvents ) -> Memory<I64>`

Allocate a memory buffer large enough to hold the specified number of epoll_event structures. Each event occupies EPOLL_EVENT_SIZE bytes.

**Parameters**:

- `maxEvents` (`I64`)
- `The maximum number of events the buffer should accommodate.`

**Returns**: — Memory<I64>:
A newly allocated buffer sized for the requested event count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

