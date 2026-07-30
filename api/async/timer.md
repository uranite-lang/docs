# uranite.async.timer

## Table of Contents

- [Imports](#imports)
- [const `SYS_TIMERFD_CREATE`](#const-sys-timerfd-create)
- [const `SYS_TIMERFD_SETTIME`](#const-sys-timerfd-settime)
- [const `CLOCK_MONOTONIC`](#const-clock-monotonic)
- [const `TFD_CLOEXEC`](#const-tfd-cloexec)
- [function `timerfdCreate`](#function-timerfdcreate)
  - [`timerfdCreate()`](#timerfdCreate)
- [function `timerfdSettime`](#function-timerfdsettime)
  - [`timerfdSettime()`](#timerfdSettime)
- [function `timerfdClose`](#function-timerfdclose)
  - [`timerfdClose()`](#timerfdClose)

## Imports

- `uranite.async.errors`
  - `AsyncRuntimeError`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `sysClose`
  - `sysRead`
  - `writeI64At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall2`
  - `syscall4`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `SYS_TIMERFD_CREATE`

Syscall number for timerfd_create on x86-64 Linux. 

## const `SYS_TIMERFD_SETTIME`

Syscall number for timerfd_settime on x86-64 Linux. 

## const `CLOCK_MONOTONIC`

Clock source for monotonic time, unaffected by system clock adjustments. 

## const `TFD_CLOEXEC`

Flag to automatically close the timerfd on exec. 

## function `timerfdCreate`

Create a new timer file descriptor using the monotonic clock. The returned fd fires readable events when the timer expires.

**Returns**: `I64` — The file descriptor for the new timer.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the timerfd_create syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function timerfdCreate(  ) -> I64`

Create a new timer file descriptor using the monotonic clock. The returned fd fires readable events when the timer expires.

**Returns**: `I64` — The file descriptor for the new timer.

**Raises**:

- `AsyncRuntimeError` → `Error` — If the timerfd_create syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `timerfdSettime`

Arm a timer file descriptor to fire once after the specified delay. Converts milliseconds to the itimerspec format (seconds + nanoseconds) and writes it via the timerfd_settime syscall.

**Parameters**:

- `fd` (`I64`)
- `The timer file descriptor to arm.`
- `milliseconds` (`I64`)
- `Delay in milliseconds before the timer fires.`

**Raises**:

- `AsyncRuntimeError` → `Error` — If the timerfd_settime syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function timerfdSettime( I64 fd, I64 milliseconds ) -> Void`

Arm a timer file descriptor to fire once after the specified delay. Converts milliseconds to the itimerspec format (seconds + nanoseconds) and writes it via the timerfd_settime syscall.

**Parameters**:

- `fd` (`I64`)
- `The timer file descriptor to arm.`
- `milliseconds` (`I64`)
- `Delay in milliseconds before the timer fires.`

**Raises**:

- `AsyncRuntimeError` → `Error` — If the timerfd_settime syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `timerfdClose`

Close a timer file descriptor, releasing its kernel resources.

**Parameters**:

- `fd` (`I64`)
- `The timer file descriptor to close.`

### Methods

#### `function timerfdClose( I64 fd ) -> Void`

Close a timer file descriptor, releasing its kernel resources.

**Parameters**:

- `fd` (`I64`)
- `The timer file descriptor to close.`

