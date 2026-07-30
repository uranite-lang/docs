# uranite.subprocess.subprocess

## Table of Contents

- [Imports](#imports)
- [const `SYS_FORK`](#const-sys-fork)
- [const `SYS_EXECVE`](#const-sys-execve)
- [const `SYS_WAIT4`](#const-sys-wait4)
- [const `SYS_EXIT_GROUP`](#const-sys-exit-group)
- [const `SYS_DUP2`](#const-sys-dup2)
- [class `SubprocessError`](#class-subprocesserror)
  - [`SubprocessError()`](#SubprocessError)
- [class `SubprocessResult`](#class-subprocessresult)
  - [`SubprocessResult()`](#SubprocessResult)
  - [`isSuccess()`](#isSuccess)
- [class `Subprocess`](#class-subprocess)
  - [`writeStdin()`](#writeStdin)
  - [`closeStdin()`](#closeStdin)
  - [`readStdout()`](#readStdout)
  - [`readStdout()`](#readStdout)
  - [`readStderr()`](#readStderr)
  - [`readStderr()`](#readStderr)
  - [`wait()`](#wait)
  - [`communicate()`](#communicate)
  - [`kill()`](#kill)
- [function `run`](#function-run)
  - [`run()`](#run)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readByteAt`
  - `readI32At`
  - `stringLen`
  - `stringToPtr`
  - `sysClose`
  - `sysPipe2`
  - `sysRead`
  - `sysWrite`
  - `writeByteAt`
  - `writeI32At`
  - `writeI64At`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall0`
  - `syscall1`
  - `syscall2`
  - `syscall3`
  - `syscall4`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `SYS_FORK`

Linux syscall number for fork. 

## const `SYS_EXECVE`

Linux syscall number for execve. 

## const `SYS_WAIT4`

Linux syscall number for wait4. 

## const `SYS_EXIT_GROUP`

Linux syscall number for exit_group. 

## const `SYS_DUP2`

Linux syscall number for dup2. 

## class `SubprocessError`

**Extends**: `Error`

Raised when a subprocess operation fails.

Covers failures in fork, execve, wait, kill, and pipe I/O operations related to subprocess management.

### Methods

#### `function SubprocessError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a new SubprocessError.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the subprocess failure.`
- `code` (`I64`)
- `Numeric error code, typically the negated errno from the`
- `failed syscall.`
- `cause` (`Error`)
- `Optional underlying error that caused this failure, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `SubprocessResult`

Immutable result of a completed subprocess execution.

Contains the exit code and captured standard output and standard error streams from the subprocess.

### Fields

| Name | Type | Access |
|------|------|--------|
| `exitCode` | `I64` | public |
| `stdout` | `String` | public |
| `stderr` | `String` | public |

### Methods

#### `function SubprocessResult( self, I64 exitCode, String stdout, String stderr ) -> Void`

Construct a new SubprocessResult with the given outputs.

**Parameters**:

- `exitCode` (`I64`)
- `The exit code returned by the subprocess.`
- `stdout` (`String`)
- `The captured standard output.`
- `stderr` (`String`)
- `The captured standard error.`

#### `function isSuccess( self ) -> Boolean`

Check whether the subprocess exited successfully.

**Returns**: — Boolean:
True if the exit code is 0, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Subprocess`

Handle for a running subprocess with stdin/stdout/stderr pipe access.

Wraps a child process created via fork+execve with connected pipes for standard I/O. Provides methods to write to the child's stdin, read from its stdout and stderr, wait for termination, communicate (close stdin then read all output and wait), and forcefully kill the process. All operations use raw Linux syscalls with zero C runtime dependency.

### Fields

| Name | Type | Access |
|------|------|--------|
| `pid` | `I64` | protect |
| `stdinWriteFd` | `I64` | protect |
| `stdoutReadFd` | `I64` | protect |
| `stderrReadFd` | `I64` | protect |
| `running` | `Boolean` | public |

### Methods

#### `function Subprocess( self, I64 pid, I64 stdinWriteFd, I64 stdoutReadFd, I64 stderrReadFd ) -> Void`

Construct a new Subprocess handle for an already-forked child process.

**Parameters**:

- `pid` (`I64`)
- `The process ID of the child.`
- `stdinWriteFd` (`I64`)
- `File descriptor for writing to the child's stdin.`
- `stdoutReadFd` (`I64`)
- `File descriptor for reading the child's stdout.`
- `stderrReadFd` (`I64`)
- `File descriptor for reading the child's stderr.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function writeStdin( self, String data ) -> Void`

Write data to the child process's standard input.

**Parameters**:

- `data` (`String`)
- `The string data to send to the child's stdin.`

#### `function closeStdin( self ) -> Void`

Close the write end of the stdin pipe.

Signals EOF to the child process's standard input. Must be called before reading output if the child reads stdin to completion.

#### `function readStdout( self ) -> String`

Read all available data from the child's standard output using a 64 KB buffer. Blocks until data is available or EOF.

**Returns**: — String:
The data read from stdout, or an empty string on EOF.

**Complexity**:
- Time: `O(n) where n is bytes available`
- Space: `O(n)`

#### `function readStdout( self, I64 maxLen ) -> String`

Read up to maxLen bytes from the child's standard output.

Blocks until data is available or the child closes its stdout.

**Parameters**:

- `maxLen` (`I64`)
- `Maximum number of bytes to read.`

**Returns**: — String:
The data read from stdout, or an empty string on EOF.

**Complexity**:
- Time: `O(1)`
- Space: `O(maxLen)`

#### `function readStderr( self ) -> String`

Read all available data from the child's standard error using a 64 KB buffer. Blocks until data is available or EOF.

**Returns**: — String:
The data read from stderr, or an empty string on EOF.

**Complexity**:
- Time: `O(n) where n is bytes available`
- Space: `O(n)`

#### `function readStderr( self, I64 maxLen ) -> String`

Read up to maxLen bytes from the child's standard error.

Blocks until data is available or the child closes its stderr.

**Parameters**:

- `maxLen` (`I64`)
- `Maximum number of bytes to read.`

**Returns**: — String:
The data read from stderr, or an empty string on EOF.

**Complexity**:
- Time: `O(1)`
- Space: `O(maxLen)`

#### `function wait( self ) -> I64`

Block until the child process terminates and return its exit code.

Uses the wait4 syscall to wait for the specific child PID. Extracts the exit code from the raw wait status using WEXITSTATUS semantics.

**Returns**: `I64` — The exit code of the child process.

**Raises**:

- `SubprocessError` → `Error` — If the wait4 syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function communicate( self ) -> SubprocessResult`

Close stdin, read all output, wait for termination, and return results.

Convenience method that performs the standard communicate pattern: closes the stdin pipe to signal EOF, reads up to 64 KiB from both stdout and stderr, waits for the child to exit, closes the output pipe file descriptors, and returns a SubprocessResult.

**Returns**: — SubprocessResult:
The exit code and captured stdout/stderr of the child process.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function kill( self ) -> Void`

Forcefully terminate the child process with SIGKILL (signal 9).

**Raises**:

- `SubprocessError` → `Error` — If the kill syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `run`

Execute a command with arguments, wait for completion, and return the captured result. Forks a child process, sets up stdin/stdout/stderr pipes, execves the command, captures all output, and waits for exit.

The arguments string is split by spaces into an argv array. The command must be an absolute path to the executable.

**Parameters**:

- `command` (`String`)
- `Absolute path to the executable` (`e.g., "/bin/ls"`)
- `arguments` (`String`)
- `Space-separated argument string` (`may be empty`)

**Returns**: `SubprocessResult` — Contains exitCode, stdout, and stderr.

**Raises**:

- `SubprocessError` → `Error` — If fork, pipe setup, or execve fails.

**Complexity**:
- Time: `O(n) where n is output size`
- Space: `O(n)`

### Methods

#### `function run( String command, String arguments ) -> SubprocessResult`

Execute a command with arguments, wait for completion, and return the captured result. Forks a child process, sets up stdin/stdout/stderr pipes, execves the command, captures all output, and waits for exit.

The arguments string is split by spaces into an argv array. The command must be an absolute path to the executable.

**Parameters**:

- `command` (`String`)
- `Absolute path to the executable` (`e.g., "/bin/ls"`)
- `arguments` (`String`)
- `Space-separated argument string` (`may be empty`)

**Returns**: `SubprocessResult` — Contains exitCode, stdout, and stderr.

**Raises**:

- `SubprocessError` → `Error` — If fork, pipe setup, or execve fails.

**Complexity**:
- Time: `O(n) where n is output size`
- Space: `O(n)`

