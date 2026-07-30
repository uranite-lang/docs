# uranite.logging.handler

## Table of Contents

- [Imports](#imports)
- [class `ConsoleHandler`](#class-consolehandler)
  - [`ConsoleHandler()`](#ConsoleHandler)
  - [`handle()`](#handle)
- [class `FileHandler`](#class-filehandler)
  - [`FileHandler()`](#FileHandler)
  - [`handle()`](#handle)
- [function `consoleHandler`](#function-consolehandler)
  - [`consoleHandler()`](#consoleHandler)
- [function `consoleHandlerWithLevel`](#function-consolehandlerwithlevel)
  - [`consoleHandlerWithLevel()`](#consoleHandlerWithLevel)
- [function `fileHandler`](#function-filehandler)
  - [`fileHandler()`](#fileHandler)
- [function `fileHandler`](#function-filehandler)
  - [`fileHandler()`](#fileHandler)
- [function `fileHandlerWithLevel`](#function-filehandlerwithlevel)
  - [`fileHandlerWithLevel()`](#fileHandlerWithLevel)
- [function `fileHandlerWithLevel`](#function-filehandlerwithlevel)
  - [`fileHandlerWithLevel()`](#fileHandlerWithLevel)

## Imports

- `uranite.io.file`
  - `File`
- `uranite.io.writer`
  - `STDERR_FD`
  - `STDOUT_FD`
  - `writeFd`
  - `writeNewlineFd`
- `uranite.logging.formatter`
  - `LogFormatter`
  - `defaultFormatter`
- `uranite.logging.level`
  - `LogLevel`

## class `ConsoleHandler`

Log handler that writes formatted messages to the console.

Messages at ERROR level or above are written to stderr, all others to stdout. Messages below the configured minimum level are silently discarded.

### Fields

| Name | Type | Access |
|------|------|--------|
| `formatter` | `LogFormatter` | public |
| `minimumLevel` | `LogLevel` | public |

### Methods

#### `function ConsoleHandler( self, LogFormatter formatter, LogLevel minimumLevel ) -> Void`

Create a new ConsoleHandler with the given formatter and minimum level.

**Parameters**:

- `formatter` (`LogFormatter`)
- `The formatter used to render log records.`
- `minimumLevel` (`LogLevel`)
- `The minimum log level threshold.`

#### `function handle( self, LogLevel level, String name, String message ) -> Void`

Format and write a log message to the console. ERROR and FATAL messages are written to stderr, all others to stdout.

**Parameters**:

- `level` (`LogLevel`)
- `The severity level of this log message.`
- `name` (`String`)
- `The logger name or context string.`
- `message` (`String`)
- `The log message text.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `FileHandler`

Log handler that writes formatted messages to a file descriptor.

All messages at or above the configured minimum level are written to the specified file descriptor regardless of severity.

### Fields

| Name | Type | Access |
|------|------|--------|
| `formatter` | `LogFormatter` | public |
| `minimumLevel` | `LogLevel` | public |
| `fileDescriptor` | `I64` | public |

### Methods

#### `function FileHandler( self, LogFormatter formatter, LogLevel minimumLevel, File file ) -> Void`

Create a new FileHandler with the given formatter, level, and File handle.

**Parameters**:

- `formatter` (`LogFormatter`)
- `The formatter used to render log records.`
- `minimumLevel` (`LogLevel`)
- `The minimum log level threshold.`
- `file` (`File`)
- `The file to write log output to.`

#### `function FileHandler( self, LogFormatter formatter, LogLevel minimumLevel, I64 fileDescriptor ) -> Void`

Create a new FileHandler with the given formatter, level, and file descriptor.

**Parameters**:

- `formatter` (`LogFormatter`)
- `The formatter used to render log records.`
- `minimumLevel` (`LogLevel`)
- `The minimum log level threshold.`
- `fileDescriptor` (`I64`)
- `The file descriptor to write log output to.`

#### `function handle( self, LogLevel level, String name, String message ) -> Void`

Format and write a log message to the configured file descriptor.

**Parameters**:

- `level` (`LogLevel`)
- `The severity level of this log message.`
- `name` (`String`)
- `The logger name or context string.`
- `message` (`String`)
- `The log message text.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `consoleHandler`

Create a new ConsoleHandler with default formatter and Trace level (all messages).

**Returns**: — ConsoleHandler:
A new console handler accepting all log levels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function consoleHandler(  ) -> ConsoleHandler`

Create a new ConsoleHandler with default formatter and Trace level (all messages).

**Returns**: — ConsoleHandler:
A new console handler accepting all log levels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `consoleHandlerWithLevel`

Create a new ConsoleHandler with default formatter and the given minimum level.

**Parameters**:

- `level` (`LogLevel`)
- `The minimum log level threshold.`

**Returns**: — ConsoleHandler:
A new console handler filtering by the given level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function consoleHandlerWithLevel( LogLevel level ) -> ConsoleHandler`

Create a new ConsoleHandler with default formatter and the given minimum level.

**Parameters**:

- `level` (`LogLevel`)
- `The minimum log level threshold.`

**Returns**: — ConsoleHandler:
A new console handler filtering by the given level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fileHandler`

Create a new FileHandler for a File with default formatter and Trace level.

**Parameters**:

- `file` (`File`)
- `The file to write log output to.`

**Returns**: — FileHandler:
A new file handler accepting all log levels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fileHandler( File file ) -> FileHandler`

Create a new FileHandler for a File with default formatter and Trace level.

**Parameters**:

- `file` (`File`)
- `The file to write log output to.`

**Returns**: — FileHandler:
A new file handler accepting all log levels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fileHandler`

Create a new FileHandler with default formatter and Trace level (all messages).

**Parameters**:

- `fileDescriptor` (`I64`)
- `The file descriptor to write log output to.`

**Returns**: — FileHandler:
A new file handler accepting all log levels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fileHandler( I64 fileDescriptor ) -> FileHandler`

Create a new FileHandler with default formatter and Trace level (all messages).

**Parameters**:

- `fileDescriptor` (`I64`)
- `The file descriptor to write log output to.`

**Returns**: — FileHandler:
A new file handler accepting all log levels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fileHandlerWithLevel`

Create a new FileHandler for a File with the given minimum level.

**Parameters**:

- `file` (`File`)
- `The file to write log output to.`
- `level` (`LogLevel`)
- `The minimum log level threshold.`

**Returns**: — FileHandler:
A new file handler filtering by the given level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fileHandlerWithLevel( File file, LogLevel level ) -> FileHandler`

Create a new FileHandler for a File with the given minimum level.

**Parameters**:

- `file` (`File`)
- `The file to write log output to.`
- `level` (`LogLevel`)
- `The minimum log level threshold.`

**Returns**: — FileHandler:
A new file handler filtering by the given level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fileHandlerWithLevel`

Create a new FileHandler with default formatter and the given minimum level.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The file descriptor to write log output to.`
- `level` (`LogLevel`)
- `The minimum log level threshold.`

**Returns**: — FileHandler:
A new file handler filtering by the given level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fileHandlerWithLevel( I64 fileDescriptor, LogLevel level ) -> FileHandler`

Create a new FileHandler with default formatter and the given minimum level.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The file descriptor to write log output to.`
- `level` (`LogLevel`)
- `The minimum log level threshold.`

**Returns**: — FileHandler:
A new file handler filtering by the given level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

