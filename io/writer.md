# uranite.io.writer

## Table of Contents

- [Imports](#imports)
- [const `STDIN_FD`](#const-stdin-fd)
- [const `STDOUT_FD`](#const-stdout-fd)
- [const `STDERR_FD`](#const-stderr-fd)
- [function `writeFd`](#function-writefd)
  - [`writeFd()`](#writeFd)
- [function `writeNewlineFd`](#function-writenewlinefd)
  - [`writeNewlineFd()`](#writeNewlineFd)
- [function `writeLineFd`](#function-writelinefd)
  - [`writeLineFd()`](#writeLineFd)
- [function `writeRawFd`](#function-writerawfd)
  - [`writeRawFd()`](#writeRawFd)

## Imports

- `uranite.io.syscall`
  - `stringLen`
  - `stringToPtr`
  - `sysWrite`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`

## const `STDIN_FD`

Standard file descriptor for standard input (fd 0). 

## const `STDOUT_FD`

Standard file descriptor for standard output (fd 1). 

## const `STDERR_FD`

Standard file descriptor for standard error (fd 2). 

## function `writeFd`

Write all bytes of a string to the given file descriptor. Handles partial writes by looping until the full string is flushed.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor` (`e.g., STDOUT_FD, STDERR_FD`)
- `content` (`String`)
- `The string whose bytes will be written.`

**Complexity**:
- Time: `O(n) where n is the byte length of content.`

### Methods

#### `function writeFd( I64 fileDescriptor, String content ) -> Void`

Write all bytes of a string to the given file descriptor. Handles partial writes by looping until the full string is flushed.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor` (`e.g., STDOUT_FD, STDERR_FD`)
- `content` (`String`)
- `The string whose bytes will be written.`

**Complexity**:
- Time: `O(n) where n is the byte length of content.`

## function `writeNewlineFd`

Write a single newline byte (0x0A) to the given file descriptor.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1) for the single-byte buffer allocation.`

### Methods

#### `function writeNewlineFd( I64 fileDescriptor ) -> Void`

Write a single newline byte (0x0A) to the given file descriptor.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1) for the single-byte buffer allocation.`

## function `writeLineFd`

Write a string followed by a newline to the given file descriptor.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor.`
- `content` (`String`)
- `The string to write before the newline.`

**Complexity**:
- Time: `O(n) where n is the byte length of content.`

### Methods

#### `function writeLineFd( I64 fileDescriptor, String content ) -> Void`

Write a string followed by a newline to the given file descriptor.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor.`
- `content` (`String`)
- `The string to write before the newline.`

**Complexity**:
- Time: `O(n) where n is the byte length of content.`

## function `writeRawFd`

Write raw bytes from a memory pointer to the given file descriptor. Handles partial writes by looping until all bytes are flushed.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor.`
- `pointer` (`I64`)
- `The memory address of the first byte to write.`
- `length` (`I64`)
- `The number of bytes to write.`

**Complexity**:
- Time: `O(length)`

### Methods

#### `function writeRawFd( I64 fileDescriptor, I64 pointer, I64 length ) -> Void`

Write raw bytes from a memory pointer to the given file descriptor. Handles partial writes by looping until all bytes are flushed.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor.`
- `pointer` (`I64`)
- `The memory address of the first byte to write.`
- `length` (`I64`)
- `The number of bytes to write.`

**Complexity**:
- Time: `O(length)`

