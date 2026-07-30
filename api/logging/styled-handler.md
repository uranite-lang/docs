# uranite.logging.styled-handler

## Table of Contents

- [Imports](#imports)
- [class `StyledConsoleHandler`](#class-styledconsolehandler)
  - [`StyledConsoleHandler()`](#StyledConsoleHandler)
  - [`StyledConsoleHandler()`](#StyledConsoleHandler)
  - [`handle()`](#handle)
  - [`setMinLevel()`](#setMinLevel)
  - [`setFormatter()`](#setFormatter)

## Imports

- `uranite.adelia.ansi-formatter`
  - `AnsiFormatter`
- `uranite.io.writer`
  - `STDERR_FD`
  - `writeFd`
  - `writeNewlineFd`
- `uranite.logging.formatter`
  - `LogFormatter`
  - `defaultFormatter`
- `uranite.logging.level`
  - `LogLevel`

## class `StyledConsoleHandler`

Console log handler with ANSI color styling via Adelia. Maps log levels to colors for visual severity differentiation: Trace  -> gray (128,128,128) Debug  -> cyan (0,188,212) Info   -> green (76,175,80) Warn   -> yellow (255,193,7) Error  -> red (244,67,54) Fatal  -> bright red (255,23,68)

### Fields

| Name | Type | Access |
|------|------|--------|
| `formatter` | `LogFormatter` | public |
| `minLevel` | `LogLevel` | public |
| `ansiFormatter` | `AnsiFormatter` | public |

### Methods

#### `function StyledConsoleHandler( self ) -> Void`

Construct handler with default formatter and Trace minimum level.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function StyledConsoleHandler( self, LogFormatter formatter, LogLevel minimumLevel ) -> Void`

Construct handler with custom formatter and minimum level.

**Parameters**:

- `formatter` (`LogFormatter`)
- `Log message formatter.`
- `minimumLevel` (`LogLevel`)
- `Minimum severity level to emit.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function handle( self, LogLevel level, String name, String message ) -> Void`

Format and write a color-styled log line to stderr.

**Parameters**:

- `level` (`LogLevel`)
- `Severity level of this log entry.`
- `name` (`String`)
- `Logger name / source identifier.`
- `message` (`String`)
- `Log message content.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setMinLevel( self, LogLevel level ) -> Void`

Update minimum severity level.

**Parameters**:

- `level` (`LogLevel`)
- `New minimum level.`

#### `function setFormatter( self, LogFormatter formatter ) -> Void`

Replace the log message formatter.

**Parameters**:

- `formatter` (`LogFormatter`)
- `New formatter to use.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function colorForLevel( self, LogLevel level, String text ) -> String`

Apply ANSI color to text based on log severity level.

**Parameters**:

- `level` (`LogLevel`)
- `Severity level determining color.`
- `text` (`String`)
- `Text to colorize.`

**Returns**: `String` — ANSI-colored text.

