# uranite.process.pipe

## Table of Contents

- [Imports](#imports)
- [class `PipeError`](#class-pipeerror)
  - [`PipeError()`](#PipeError)
- [class `Pipe`](#class-pipe)
  - [`Pipe()`](#Pipe)
  - [`read()`](#read)
  - [`write()`](#write)
  - [`closeRead()`](#closeRead)
  - [`closeWrite()`](#closeWrite)
  - [`closeAll()`](#closeAll)

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
- `uranite.memory.allocator`
  - `alloc`
- `uranite.memory.memory`
  - `Memory`

## class `PipeError`

**Extends**: `Error`

Raised when a pipe operation fails.

Covers failures in pipe creation, reading, writing, or closing pipe file descriptors.

### Methods

#### `function PipeError( self, String message, I64 code, ?Error cause ) -> Void`

Construct a new PipeError.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the pipe failure.`
- `code` (`I64`)
- `Numeric error code identifying the failure type.`
- `cause` (`Error`)
- `Optional underlying error that caused this failure, or None.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Pipe`

Unidirectional byte stream for inter-process communication.

Wraps the Linux pipe2 syscall to create a pair of connected file descriptors. Data written to the write end can be read from the read end. Typically used to connect the output of one process to the input of another, or to capture subprocess output.

### Fields

| Name | Type | Access |
|------|------|--------|
| `readFd` | `I64` | public |
| `writeFd` | `I64` | public |

### Methods

#### `function Pipe( self ) -> Void`

Create a new pipe by invoking the pipe2 syscall.

Allocates a pair of connected file descriptors and stores them in readFd and writeFd. The pipe is created with default flags.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function read( self, I64 maxLen ) -> String`

Read up to maxLen bytes from the read end of the pipe.

Blocks until data is available or the write end is closed. Returns an empty string if no data was read (write end closed or error).

**Parameters**:

- `maxLen` (`I64`)
- `Maximum number of bytes to read.`

**Returns**: — String:
The data read from the pipe, or an empty string on EOF.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function write( self, String data ) -> I64`

Write data to the write end of the pipe.

**Parameters**:

- `data` (`String`)
- `The string data to write into the pipe.`

**Returns**: `I64` — The number of bytes actually written, or a negative value on error.

#### `function closeRead( self ) -> Void`

Close the read end of the pipe.

After closing, no further reads can be performed. If the write end is still open, writers will receive a broken pipe signal.

#### `function closeWrite( self ) -> Void`

Close the write end of the pipe.

After closing, readers will receive EOF once all buffered data has been consumed.

#### `function closeAll( self ) -> Void`

Close both the read and write ends of the pipe.

Releases both file descriptors. The pipe becomes unusable after this call.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

