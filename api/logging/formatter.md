# uranite.logging.formatter

## Table of Contents

- [Imports](#imports)
- [const `DEFAULT_PATTERN`](#const-default-pattern)
- [const `SIMPLE_PATTERN`](#const-simple-pattern)
- [const `DETAILED_PATTERN`](#const-detailed-pattern)
- [const `CLASSIC_PATTERN`](#const-classic-pattern)
- [const `SLF4J_PATTERN`](#const-slf4j-pattern)
- [function `matchPlaceholder`](#function-matchplaceholder)
- [class `LogFormatter`](#class-logformatter)
  - [`LogFormatter()`](#LogFormatter)
  - [`setUsername()`](#setUsername)
  - [`setProgram()`](#setProgram)
  - [`setUtcOffset()`](#setUtcOffset)
  - [`format()`](#format)
- [function `defaultFormatter`](#function-defaultformatter)
  - [`defaultFormatter()`](#defaultFormatter)
- [function `simpleFormatter`](#function-simpleformatter)
  - [`simpleFormatter()`](#simpleFormatter)
- [function `detailedFormatter`](#function-detailedformatter)
  - [`detailedFormatter()`](#detailedFormatter)
- [function `classicFormatter`](#function-classicformatter)
  - [`classicFormatter()`](#classicFormatter)
- [function `slf4jFormatter`](#function-slf4jformatter)
  - [`slf4jFormatter()`](#slf4jFormatter)

## Imports

- `uranite.convert.convert`
  - `intToString`
- `uranite.datetime.datetime`
  - `DateTime`
  - `now`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.logging.level`
  - `LogLevel`
- `uranite.memory.allocator`
  - `alloc`
- `uranite.string.format`
  - `appendTo`
  - `formatUtcOffset`
  - `padRight`
  - `padZero`
- `uranite.sys.process`
  - `getUsername`
  - `getpid`

## const `DEFAULT_PATTERN`

Default log format pattern matching the custom logger output style. 

## const `SIMPLE_PATTERN`

Minimal log format pattern with only level and message. 

## const `DETAILED_PATTERN`

Bracketed log format pattern with timestamp, level, name, and message. 

## const `CLASSIC_PATTERN`

Classic log format pattern similar to Log4j/Logback default. 

## const `SLF4J_PATTERN`

SLF4J-style log format pattern with thread, padded level, and name. 

## function `matchPlaceholder`

Check whether a placeholder string matches at the given position in the pattern buffer.

**Parameters**:

- `patternPointer` (`I64`)
- `The memory address of the pattern string bytes.`
- `position` (`I64`)
- `The current byte offset to check from.`
- `patternLength` (`I64`)
- `The total byte length of the pattern string.`
- `placeholder` (`String`)
- `The placeholder string to match` (`e.g., "{level}"`)

**Returns**: — Boolean:
True if the placeholder matches at position, False otherwise.

**Complexity**:
- Time: `O(m) where m is the byte length of placeholder.`

### Methods

#### `function matchPlaceholder( I64 patternPointer, I64 position, I64 patternLength, String placeholder ) -> Boolean`

Check whether a placeholder string matches at the given position in the pattern buffer.

**Parameters**:

- `patternPointer` (`I64`)
- `The memory address of the pattern string bytes.`
- `position` (`I64`)
- `The current byte offset to check from.`
- `patternLength` (`I64`)
- `The total byte length of the pattern string.`
- `placeholder` (`String`)
- `The placeholder string to match` (`e.g., "{level}"`)

**Returns**: — Boolean:
True if the placeholder matches at position, False otherwise.

**Complexity**:
- Time: `O(m) where m is the byte length of placeholder.`

## class `LogFormatter`

Pattern-based log message formatter with configurable placeholders.

Resolves placeholder tokens in a pattern string with runtime values such as timestamps, log levels, process IDs, and usernames. The pattern is set at construction time and evaluated on each call to format().
 Supported placeholders: {datetime}, {utcoffset}, {timestamp}, {thread}, {level}, {level5}, {level7}, {username}, {pid}, {program}, {program16}, {name}, {name40}, {message}.

### Fields

| Name | Type | Access |
|------|------|--------|
| `pattern` | `String` | public |
| `username` | `String` | public |
| `program` | `String` | public |
| `utcOffsetSeconds` | `I64` | public |

### Methods

#### `function LogFormatter( self, String pattern ) -> Void`

Create a new LogFormatter with the given pattern string.

**Parameters**:

- `pattern` (`String`)
- `Format pattern with placeholders like {datetime}, {level}, {message}.`

#### `function setUsername( self, String username ) -> Void`

Override the OS username shown in formatted output.

**Parameters**:

- `username` (`String`)
- `The username string to display.`

#### `function setProgram( self, String program ) -> Void`

Set the program identifier shown in formatted output.

**Parameters**:

- `program` (`String`)
- `The program name.`

#### `function setUtcOffset( self, I64 offsetSeconds ) -> Void`

Set the UTC offset used for timestamp formatting.

**Parameters**:

- `offsetSeconds` (`I64`)
- `The UTC offset in seconds` (`e.g., 25200 for +07:00`)

#### `function format( self, LogLevel level, String name, String message ) -> String`

Format a log record into a complete log line string by resolving all placeholders in self.pattern with runtime values.

**Parameters**:

- `level` (`LogLevel`)
- `The log level enum value.`
- `name` (`String`)
- `The logger name or context string.`
- `message` (`String`)
- `The log message text.`

**Returns**: — String:
The fully formatted log line string.

**Complexity**:
- Time: `O(n) where n is the pattern length.`
- Space: `O(n) for the output buffer allocation.`

## function `defaultFormatter`

Create a new LogFormatter with the default pattern.

**Returns**: — LogFormatter:
A new formatter using DEFAULT_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function defaultFormatter(  ) -> LogFormatter`

Create a new LogFormatter with the default pattern.

**Returns**: — LogFormatter:
A new formatter using DEFAULT_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `simpleFormatter`

Create a new LogFormatter with the simple pattern.

**Returns**: — LogFormatter:
A new formatter using SIMPLE_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function simpleFormatter(  ) -> LogFormatter`

Create a new LogFormatter with the simple pattern.

**Returns**: — LogFormatter:
A new formatter using SIMPLE_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `detailedFormatter`

Create a new LogFormatter with the detailed pattern.

**Returns**: — LogFormatter:
A new formatter using DETAILED_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function detailedFormatter(  ) -> LogFormatter`

Create a new LogFormatter with the detailed pattern.

**Returns**: — LogFormatter:
A new formatter using DETAILED_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `classicFormatter`

Create a new LogFormatter with the classic pattern.

**Returns**: — LogFormatter:
A new formatter using CLASSIC_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function classicFormatter(  ) -> LogFormatter`

Create a new LogFormatter with the classic pattern.

**Returns**: — LogFormatter:
A new formatter using CLASSIC_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `slf4jFormatter`

Create a new LogFormatter with the SLF4J-style pattern.

**Returns**: — LogFormatter:
A new formatter using SLF4J_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function slf4jFormatter(  ) -> LogFormatter`

Create a new LogFormatter with the SLF4J-style pattern.

**Returns**: — LogFormatter:
A new formatter using SLF4J_PATTERN.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

