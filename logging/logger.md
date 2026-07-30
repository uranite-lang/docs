# uranite.logging.logger

## Table of Contents

- [Imports](#imports)
- [class `Logger`](#class-logger)
  - [`Logger()`](#Logger)
  - [`setLevel()`](#setLevel)
  - [`setOutput()`](#setOutput)
  - [`setOutput()`](#setOutput)
  - [`setUsername()`](#setUsername)
  - [`setProgram()`](#setProgram)
  - [`setUtcOffset()`](#setUtcOffset)
  - [`isEnabled()`](#isEnabled)
  - [`log()`](#log)
  - [`trace()`](#trace)
  - [`debug()`](#debug)
  - [`info()`](#info)
  - [`warn()`](#warn)
  - [`error()`](#error)
  - [`fatal()`](#fatal)
  - [`logStructured()`](#logStructured)
- [function `getLogger`](#function-getlogger)
  - [`getLogger()`](#getLogger)
- [function `getDebugLogger`](#function-getdebuglogger)
  - [`getDebugLogger()`](#getDebugLogger)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.collection.hash-map`
  - `HashMap`
- `uranite.collection.hash-set`
  - `HashSet`
- `uranite.convert.convert`
  - `intToString`
- `uranite.datetime.datetime`
  - `DateTime`
  - `now`
- `uranite.io.file`
  - `File`
- `uranite.io.writer`
  - `STDERR_FD`
  - `STDOUT_FD`
  - `writeFd`
  - `writeNewlineFd`
- `uranite.logging.level`
  - `LogLevel`
- `uranite.string.format`
  - `formatUtcOffset`
  - `padRight`
  - `padZero`
- `uranite.sys.process`
  - `getUsername`
  - `getpid`

## class `Logger`

Named logger with configurable level threshold and formatted output.

Each Logger instance has a name, a minimum log level, and an output file descriptor. Messages below the configured level are silently discarded. ERROR and FATAL messages are automatically redirected to stderr regardless of the configured output file descriptor.
 Output format matches the custom pattern: {datetime}{utcoffset}   {level}   {username}  P{pid}:T0    --- [{program}] {name} : {message}

### Fields

| Name | Type | Access |
|------|------|--------|
| `name` | `String` | public |
| `level` | `LogLevel` | public |
| `outputFileDescriptor` | `I64` | public |
| `username` | `String` | public |
| `program` | `String` | public |
| `utcOffsetSeconds` | `I64` | public |

### Methods

#### `function Logger( self, String name, LogLevel level ) -> Void`

Create a new Logger with the given name and minimum log level.

**Parameters**:

- `name` (`String`)
- `The logger name, typically the fully qualified module or class name.`
- `level` (`LogLevel`)
- `The minimum log level threshold` (`e.g., LogLevel.Info, LogLevel.Debug`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setLevel( self, LogLevel level ) -> Void`

Set the minimum log level threshold.

**Parameters**:

- `level` (`LogLevel`)
- `The new minimum level. Messages below this are discarded.`

#### `function setOutput( self, File file ) -> Void`

Set the output to a File handle for log messages.

**Parameters**:

- `file` (`File`)
- `The target file to write log output to.`

#### `function setOutput( self, I64 fileDescriptor ) -> Void`

Set the output file descriptor for log messages.

**Parameters**:

- `fileDescriptor` (`I64`)
- `The target file descriptor` (`e.g., STDOUT_FD, STDERR_FD`)

#### `function setUsername( self, String username ) -> Void`

Override the OS username shown in log output.

**Parameters**:

- `username` (`String`)
- `The username string to display.`

#### `function setProgram( self, String program ) -> Void`

Set the program identifier shown in log output.

**Parameters**:

- `program` (`String`)
- `The program name, padded to 16 characters in output.`

#### `function setUtcOffset( self, I64 offsetSeconds ) -> Void`

Set the UTC offset used for timestamp formatting.

**Parameters**:

- `offsetSeconds` (`I64`)
- `The UTC offset in seconds` (`e.g., 25200 for +07:00`)

#### `function isEnabled( self, LogLevel messageLevel ) -> Boolean`

Check whether a message at the given level would be logged.

**Parameters**:

- `messageLevel` (`LogLevel`)
- `The log level to check.`

**Returns**: `Boolean` — True if messageLevel is at or above the configured threshold.

#### `function log( self, LogLevel messageLevel, String message ) -> Void`

Log a message at the given level. Messages below the configured threshold are silently discarded. ERROR and FATAL messages are automatically redirected to stderr.

**Parameters**:

- `messageLevel` (`LogLevel`)
- `The severity level of this message.`
- `message` (`String`)
- `The log message text.`

**Complexity**:
- Time: `O(n) where n is the total output line length.`

#### `function trace( self, String message ) -> Void`

Log a message at TRACE level. 

#### `function debug( self, String message ) -> Void`

Log a message at DEBUG level. 

#### `function info( self, String message ) -> Void`

Log a message at INFO level. 

#### `function warn( self, String message ) -> Void`

Log a message at WARN level. 

#### `function error( self, String message ) -> Void`

Log a message at ERROR level. 

#### `function fatal( self, String message ) -> Void`

Log a message at FATAL level. 

#### `function logStructured( self, LogLevel messageLevel, String message, HashMap<String, String> fields ) -> Void`

Log a message with key-value metadata fields appended.

The fields are formatted as key=value pairs separated by spaces and appended after the message text. Useful for structured logging where downstream systems parse field values.

**Parameters**:

- `messageLevel` (`LogLevel`)
- `The severity level of the log entry.`
- `message` (`String`)
- `The primary log message text.`
- `fields` (`HashMap<String, String>`)
- `Key-value metadata pairs to include in the log line.`

**Complexity**:
- Time: `O(n) where n is the number of fields`

## function `getLogger`

Create a new Logger with INFO as the default level.

**Parameters**:

- `name` (`String`)
- `The logger name, typically the fully qualified module or class name.`

**Returns**: — Logger:
A new Logger instance configured at INFO level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getLogger( String name ) -> Logger`

Create a new Logger with INFO as the default level.

**Parameters**:

- `name` (`String`)
- `The logger name, typically the fully qualified module or class name.`

**Returns**: — Logger:
A new Logger instance configured at INFO level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `getDebugLogger`

Create a new Logger with DEBUG as the default level.

**Parameters**:

- `name` (`String`)
- `The logger name, typically the fully qualified module or class name.`

**Returns**: — Logger:
A new Logger instance configured at DEBUG level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getDebugLogger( String name ) -> Logger`

Create a new Logger with DEBUG as the default level.

**Parameters**:

- `name` (`String`)
- `The logger name, typically the fully qualified module or class name.`

**Returns**: — Logger:
A new Logger instance configured at DEBUG level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

