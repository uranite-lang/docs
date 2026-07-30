# uranite.sys.process

## Table of Contents

- [Imports](#imports)
- [const `SYS_EXIT_GROUP`](#const-sys-exit-group)
- [const `SYS_GETPID`](#const-sys-getpid)
- [const `SYS_GETPPID`](#const-sys-getppid)
- [const `SYS_GETUID`](#const-sys-getuid)
- [const `SYS_GETGID`](#const-sys-getgid)
- [const `SYS_GETEUID`](#const-sys-geteuid)
- [const `SYS_GETEGID`](#const-sys-getegid)
- [const `SYS_FORK`](#const-sys-fork)
- [const `SYS_KILL`](#const-sys-kill)
- [const `SYS_NANOSLEEP`](#const-sys-nanosleep)
- [const `SYS_WAIT4`](#const-sys-wait4)
- [const `SYS_GETPGID`](#const-sys-getpgid)
- [const `SYS_SETSID`](#const-sys-setsid)
- [const `SIGTERM`](#const-sigterm)
- [const `SIGKILL`](#const-sigkill)
- [const `SIGINT`](#const-sigint)
- [const `SIGHUP`](#const-sighup)
- [const `SIGUSR1`](#const-sigusr1)
- [const `SIGUSR2`](#const-sigusr2)
- [class `ProcessError`](#class-processerror)
  - [`ProcessError()`](#ProcessError)
- [function `exit`](#function-exit)
  - [`exit()`](#exit)
- [function `getpid`](#function-getpid)
  - [`getpid()`](#getpid)
- [function `getppid`](#function-getppid)
  - [`getppid()`](#getppid)
- [function `getuid`](#function-getuid)
  - [`getuid()`](#getuid)
- [function `getgid`](#function-getgid)
  - [`getgid()`](#getgid)
- [function `geteuid`](#function-geteuid)
  - [`geteuid()`](#geteuid)
- [function `getegid`](#function-getegid)
  - [`getegid()`](#getegid)
- [function `fork`](#function-fork)
  - [`fork()`](#fork)
- [function `kill`](#function-kill)
  - [`kill()`](#kill)
- [function `waitForProcess`](#function-waitforprocess)
  - [`waitForProcess()`](#waitForProcess)
- [function `wait4`](#function-wait4)
  - [`wait4()`](#wait4)
- [function `getpgid`](#function-getpgid)
  - [`getpgid()`](#getpgid)
- [function `setsid`](#function-setsid)
  - [`setsid()`](#setsid)
- [function `getUsername`](#function-getusername)
  - [`getUsername()`](#getUsername)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `O_RDONLY`
  - `SYS_CLOSE`
  - `SYS_OPEN`
  - `SYS_READ`
  - `readByteAt`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
- `uranite.os.syscall.invoke`
  - `syscall0`
  - `syscall1`
  - `syscall2`
  - `syscall3`
  - `syscall4`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `SYS_EXIT_GROUP`

## const `SYS_GETPID`

## const `SYS_GETPPID`

## const `SYS_GETUID`

## const `SYS_GETGID`

## const `SYS_GETEUID`

## const `SYS_GETEGID`

## const `SYS_FORK`

## const `SYS_KILL`

## const `SYS_NANOSLEEP`

## const `SYS_WAIT4`

## const `SYS_GETPGID`

## const `SYS_SETSID`

## const `SIGTERM`

## const `SIGKILL`

## const `SIGINT`

## const `SIGHUP`

## const `SIGUSR1`

## const `SIGUSR2`

## class `ProcessError`

**Extends**: `Error`

Error raised when a process-related syscall fails (fork, kill, wait4, etc.).

### Methods

#### `function ProcessError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new ProcessError with a message, error code, and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the failure.`
- `code` (`I64`)
- `The errno value from the failed syscall.`
- `cause` (`?Error`)
- `An optional chained error that caused this one, or None.`

## function `exit`

Terminate the process with the given exit code via SYS_EXIT_GROUP.

**Parameters**:

- `code` (`I64`)
- `The exit code returned to the parent process.`

### Methods

#### `function exit( I64 code ) -> Void`

Terminate the process with the given exit code via SYS_EXIT_GROUP.

**Parameters**:

- `code` (`I64`)
- `The exit code returned to the parent process.`

## function `getpid`

Get the process ID of the calling process.

**Returns**: `I64` — The process ID.

### Methods

#### `function getpid(  ) -> I64`

Get the process ID of the calling process.

**Returns**: `I64` — The process ID.

## function `getppid`

Get the process ID of the parent process.

**Returns**: `I64` — The parent process ID.

### Methods

#### `function getppid(  ) -> I64`

Get the process ID of the parent process.

**Returns**: `I64` — The parent process ID.

## function `getuid`

Get the real user ID of the calling process.

**Returns**: `I64` — The real user ID.

### Methods

#### `function getuid(  ) -> I64`

Get the real user ID of the calling process.

**Returns**: `I64` — The real user ID.

## function `getgid`

Get the real group ID of the calling process.

**Returns**: `I64` — The real group ID.

### Methods

#### `function getgid(  ) -> I64`

Get the real group ID of the calling process.

**Returns**: `I64` — The real group ID.

## function `geteuid`

Get the effective user ID of the calling process.

**Returns**: `I64` — The effective user ID.

### Methods

#### `function geteuid(  ) -> I64`

Get the effective user ID of the calling process.

**Returns**: `I64` — The effective user ID.

## function `getegid`

Get the effective group ID of the calling process.

**Returns**: `I64` — The effective group ID.

### Methods

#### `function getegid(  ) -> I64`

Get the effective group ID of the calling process.

**Returns**: `I64` — The effective group ID.

## function `fork`

Create a child process via the fork syscall.

**Returns**: `I64` — 0 in the child process, the child's PID in the parent.

**Raises**:

- `ProcessError` → `Error` — If the fork syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fork(  ) -> I64`

Create a child process via the fork syscall.

**Returns**: `I64` — 0 in the child process, the child's PID in the parent.

**Raises**:

- `ProcessError` → `Error` — If the fork syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `kill`

Send a signal to a process.

**Parameters**:

- `processId` (`I64`)
- `The PID of the target process.`
- `signal` (`I64`)
- `The signal number to send` (`e.g., SIGTERM, SIGKILL`)

**Raises**:

- `ProcessError` → `Error` — If the kill syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function kill( I64 processId, I64 signal ) -> Void`

Send a signal to a process.

**Parameters**:

- `processId` (`I64`)
- `The PID of the target process.`
- `signal` (`I64`)
- `The signal number to send` (`e.g., SIGTERM, SIGKILL`)

**Raises**:

- `ProcessError` → `Error` — If the kill syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `waitForProcess`

Wait for a child process to terminate and return its exit status.

**Parameters**:

- `processId` (`I64`)
- `The PID of the child process to wait for.`

**Returns**: `I64` — The exit status of the child process.

**Raises**:

- `ProcessError` → `Error` — If the wait fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function waitForProcess( I64 processId ) -> I64`

Wait for a child process to terminate and return its exit status.

**Parameters**:

- `processId` (`I64`)
- `The PID of the child process to wait for.`

**Returns**: `I64` — The exit status of the child process.

**Raises**:

- `ProcessError` → `Error` — If the wait fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `wait4`

Wait for a child process to change state.

**Parameters**:

- `processId` (`I64`)
- `The PID to wait for, or -1 for any child.`
- `statusPointer` (`I64`)
- `Memory address to store the status information.`
- `options` (`I64`)
- `Wait options` (`e.g., WNOHANG`)
- `rusagePointer` (`I64`)
- `Memory address for resource usage info, or 0.`

**Returns**: `I64` — The PID of the child that changed state.

**Raises**:

- `ProcessError` → `Error` — If the wait4 syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function wait4( I64 processId, I64 statusPointer, I64 options, I64 rusagePointer ) -> I64`

Wait for a child process to change state.

**Parameters**:

- `processId` (`I64`)
- `The PID to wait for, or -1 for any child.`
- `statusPointer` (`I64`)
- `Memory address to store the status information.`
- `options` (`I64`)
- `Wait options` (`e.g., WNOHANG`)
- `rusagePointer` (`I64`)
- `Memory address for resource usage info, or 0.`

**Returns**: `I64` — The PID of the child that changed state.

**Raises**:

- `ProcessError` → `Error` — If the wait4 syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `getpgid`

Get the process group ID of the specified process.

**Parameters**:

- `processId` (`I64`)
- `The PID to query, or 0 for the calling process.`

**Returns**: `I64` — The process group ID.

**Raises**:

- `ProcessError` → `Error` — If the getpgid syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getpgid( I64 processId ) -> I64`

Get the process group ID of the specified process.

**Parameters**:

- `processId` (`I64`)
- `The PID to query, or 0 for the calling process.`

**Returns**: `I64` — The process group ID.

**Raises**:

- `ProcessError` → `Error` — If the getpgid syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `setsid`

Create a new session and set the calling process as session leader.

**Returns**: `I64` — The new session ID.

**Raises**:

- `ProcessError` → `Error` — If the setsid syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function setsid(  ) -> I64`

Create a new session and set the calling process as session leader.

**Returns**: `I64` — The new session ID.

**Raises**:

- `ProcessError` → `Error` — If the setsid syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `getUsername`

Resolve the current OS username by reading /etc/passwd and matching the UID from getuid(). Parses each line to extract the username field (field 0) and UID field (field 2, colon-delimited).

**Returns**: — String:
The username string, or "unknown" if /etc/passwd cannot be
read or the UID is not found.

**Complexity**:
- Time: `O(n) where n is the byte size of /etc/passwd.`
- Space: `O(n) for the file read buffer.`

### Methods

#### `function getUsername(  ) -> String`

Resolve the current OS username by reading /etc/passwd and matching the UID from getuid(). Parses each line to extract the username field (field 0) and UID field (field 2, colon-delimited).

**Returns**: — String:
The username string, or "unknown" if /etc/passwd cannot be
read or the UID is not found.

**Complexity**:
- Time: `O(n) where n is the byte size of /etc/passwd.`
- Space: `O(n) for the file read buffer.`

