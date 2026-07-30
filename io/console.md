# uranite.io.console

## Table of Contents

- [Imports](#imports)
- [function `writeArgsToFd`](#function-writeargstofd)
- [function `writeI64ArgsToFd`](#function-writei64argstofd)
- [function `input`](#function-input)
  - [`input()`](#input)
- [function `getpass`](#function-getpass)
  - [`getpass()`](#getpass)
- [function `puts`](#function-puts)
  - [`puts()`](#puts)
- [function `putsln`](#function-putsln)
  - [`putsln()`](#putsln)
- [function `putserr`](#function-putserr)
  - [`putserr()`](#putserr)
- [function `putserrln`](#function-putserrln)
  - [`putserrln()`](#putserrln)
- [function `putsI64`](#function-putsi64)
  - [`putsI64()`](#putsI64)

## Imports

- `uranite.convert.convert`
  - `intToString`
- `uranite.io.stream`
  - `Stream`
- `uranite.io.syscall`
  - `ECHO_FLAG`
  - `TCGETS`
  - `TCSETS`
  - `memoryToPtr`
  - `ptrToString`
  - `readByteAt`
  - `readI32At`
  - `stringLen`
  - `stringToPtr`
  - `sysIoctl`
  - `sysRead`
  - `sysWrite`
  - `writeByteAt`
  - `writeI32At`
- `uranite.io.writer`
  - `STDERR_FD`
  - `STDIN_FD`
  - `STDOUT_FD`
  - `writeFd`
  - `writeNewlineFd`
  - `writeRawFd`
- `uranite.memory.memory`
  - `Memory`

## function `writeArgsToFd`

Core write routine that all puts variants delegate to. Writes each variadic object argument to the resolved file descriptor, separated by the separator string (if not None) and followed by the endline string (if not None).

**Parameters**:

- `targetFileDescriptor` (`I64`)
- `The default file descriptor to write to when stream is None.`
- `args` (`Object[]`)
- `The variadic object arguments to write.`
- `separator` (`String`)
- `The separator written between arguments. None to suppress.`
- `endline` (`String`)
- `The string written after all arguments. None to suppress.`
- `stream` (`Stream`)
- `An optional stream override. When not None, output goes to`
- `the stream's file descriptor instead of targetFileDescriptor.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support. Currently a no-op since`
- `raw syscall writes are unbuffered.`

### Methods

#### `function writeArgsToFd( I64 targetFileDescriptor, [Object] args[], ?String separator, ?String endline, ?Stream stream, Boolean flush ) -> Void`

Core write routine that all puts variants delegate to. Writes each variadic object argument to the resolved file descriptor, separated by the separator string (if not None) and followed by the endline string (if not None).

**Parameters**:

- `targetFileDescriptor` (`I64`)
- `The default file descriptor to write to when stream is None.`
- `args` (`Object[]`)
- `The variadic object arguments to write.`
- `separator` (`String`)
- `The separator written between arguments. None to suppress.`
- `endline` (`String`)
- `The string written after all arguments. None to suppress.`
- `stream` (`Stream`)
- `An optional stream override. When not None, output goes to`
- `the stream's file descriptor instead of targetFileDescriptor.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support. Currently a no-op since`
- `raw syscall writes are unbuffered.`

## function `writeI64ArgsToFd`

Core write routine for integer arguments. Converts each I64 value to its decimal string representation before writing.

**Parameters**:

- `targetFileDescriptor` (`I64`)
- `The default file descriptor to write to when stream is None.`
- `args` (`I64[]`)
- `The variadic integer arguments to write.`
- `separator` (`String`)
- `The separator written between arguments. None to suppress.`
- `endline` (`String`)
- `The string written after all arguments. None to suppress.`
- `stream` (`Stream`)
- `An optional stream override.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

### Methods

#### `function writeI64ArgsToFd( I64 targetFileDescriptor, [I64] args[], ?String separator, ?String endline, ?Stream stream, Boolean flush ) -> Void`

Core write routine for integer arguments. Converts each I64 value to its decimal string representation before writing.

**Parameters**:

- `targetFileDescriptor` (`I64`)
- `The default file descriptor to write to when stream is None.`
- `args` (`I64[]`)
- `The variadic integer arguments to write.`
- `separator` (`String`)
- `The separator written between arguments. None to suppress.`
- `endline` (`String`)
- `The string written after all arguments. None to suppress.`
- `stream` (`Stream`)
- `An optional stream override.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

## function `input`

Display a prompt and read a line of input from stdin. The trailing newline is stripped from the returned string. Dynamically grows the read buffer if the input exceeds capacity.

**Parameters**:

- `prompt` (`String`)
- `The prompt string displayed before reading input.`

**Returns**: — String:
The line read from stdin, without the trailing newline.

**Complexity**:
- Time: `O(n) where n is the number of bytes read.`
- Space: `O(n) for the dynamically grown input buffer.`

### Methods

#### `function input( String prompt ) -> String`

Display a prompt and read a line of input from stdin. The trailing newline is stripped from the returned string. Dynamically grows the read buffer if the input exceeds capacity.

**Parameters**:

- `prompt` (`String`)
- `The prompt string displayed before reading input.`

**Returns**: — String:
The line read from stdin, without the trailing newline.

**Complexity**:
- Time: `O(n) where n is the number of bytes read.`
- Space: `O(n) for the dynamically grown input buffer.`

## function `getpass`

Display a prompt and read a password from stdin with echo disabled. Terminal echo is temporarily turned off via ioctl TCSETS, then restored after reading. The trailing newline is stripped.

**Parameters**:

- `prompt` (`String`)
- `The prompt string displayed before reading the password.`

**Returns**: — String:
The password string read from stdin, without the trailing newline.

**Complexity**:
- Time: `O(n) where n is the number of bytes read.`
- Space: `O(n) for the password buffer plus O(1) for termios state.`

### Methods

#### `function getpass( String prompt ) -> String`

Display a prompt and read a password from stdin with echo disabled. Terminal echo is temporarily turned off via ioctl TCSETS, then restored after reading. The trailing newline is stripped.

**Parameters**:

- `prompt` (`String`)
- `The prompt string displayed before reading the password.`

**Returns**: — String:
The password string read from stdin, without the trailing newline.

**Complexity**:
- Time: `O(n) where n is the number of bytes read.`
- Space: `O(n) for the password buffer plus O(1) for termios state.`

## function `puts`

Write variadic object arguments to stdout, separated by separator and followed by endline. Override the output target with stream.

**Parameters**:

- `args` (`Object[]`)
- `The strings to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stdout.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

### Methods

#### `function puts( [Object] args[], ?String separator, ?String endline, ?Stream stream, Boolean flush ) -> Void`

Write variadic object arguments to stdout, separated by separator and followed by endline. Override the output target with stream.

**Parameters**:

- `args` (`Object[]`)
- `The strings to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stdout.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

## function `putsln`

Write variadic object arguments to stdout with a trailing newline. Identical to puts with default parameters.

**Parameters**:

- `args` (`Object[]`)
- `The strings to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stdout.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

### Methods

#### `function putsln( [Object] args[], ?String separator, ?String endline, ?Stream stream, Boolean flush ) -> Void`

Write variadic object arguments to stdout with a trailing newline. Identical to puts with default parameters.

**Parameters**:

- `args` (`Object[]`)
- `The strings to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stdout.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

## function `putserr`

Write variadic object arguments to stderr, separated by separator and followed by endline.

**Parameters**:

- `args` (`Object[]`)
- `The strings to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stderr.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

### Methods

#### `function putserr( [Object] args[], ?String separator, ?String endline, ?Stream stream, Boolean flush ) -> Void`

Write variadic object arguments to stderr, separated by separator and followed by endline.

**Parameters**:

- `args` (`Object[]`)
- `The strings to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stderr.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

## function `putserrln`

Write variadic object arguments to stderr with a trailing newline. Identical to putserr with default parameters.

**Parameters**:

- `args` (`Object[]`)
- `The strings to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stderr.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

### Methods

#### `function putserrln( [Object] args[], ?String separator, ?String endline, ?Stream stream, Boolean flush ) -> Void`

Write variadic object arguments to stderr with a trailing newline. Identical to putserr with default parameters.

**Parameters**:

- `args` (`Object[]`)
- `The strings to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stderr.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

## function `putsI64`

Write variadic integer arguments to stdout as decimal strings, separated by separator and followed by endline.

**Parameters**:

- `args` (`I64[]`)
- `The integer values to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stdout.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function putsI64( [I64] args[], ?String separator, ?String endline, ?Stream stream, Boolean flush ) -> Void`

Write variadic integer arguments to stdout as decimal strings, separated by separator and followed by endline.

**Parameters**:

- `args` (`I64[]`)
- `The integer values to write.`
- `separator` (`String`)
- `Written between arguments. Default space. None to suppress.`
- `endline` (`String`)
- `Written after all arguments. Default newline. None to suppress.`
- `stream` (`Stream`)
- `Optional output target. None for stdout.`
- `flush` (`Boolean`)
- `Reserved for buffered stream support.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

