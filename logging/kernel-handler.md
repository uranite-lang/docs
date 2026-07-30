# uranite.logging.kernel-handler

## Table of Contents

- [Imports](#imports)
- [class `KernelLogHandler`](#class-kernelloghandler)
  - [`KernelLogHandler()`](#KernelLogHandler)
  - [`setMinLevel()`](#setMinLevel)
  - [`getMinLevel()`](#getMinLevel)
  - [`log()`](#log)
  - [`logTrace()`](#logTrace)
  - [`logDebug()`](#logDebug)
  - [`logInfo()`](#logInfo)
  - [`logWarn()`](#logWarn)
  - [`logError()`](#logError)
  - [`logFatal()`](#logFatal)
  - [`getCount()`](#getCount)
  - [`countAtLevel()`](#countAtLevel)
  - [`getTimestamp()`](#getTimestamp)
  - [`getLevel()`](#getLevel)
  - [`getSource()`](#getSource)
  - [`getCode()`](#getCode)
  - [`clear()`](#clear)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.debug.log`
  - `EventLog`
  - `KernelLogLevel`

## class `KernelLogHandler`

Log handler that writes entries to the kernel event log ring buffer. Each log call records a timestamp, severity level, source identifier, and event code. Entries below the configured minimum level are dropped.

### Fields

| Name | Type | Access |
|------|------|--------|
| `eventLog` | `EventLog` | protect |
| `minLevel` | `I64` | protect |
| `nextTimestamp` | `I64` | protect |

### Methods

#### `function KernelLogHandler( self, I64 capacity ) -> Void`

Construct a kernel log handler backed by a ring buffer.

**Parameters**:

- `capacity` (`I64`)
- `Maximum number of log entries before oldest is overwritten.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setMinLevel( self, I64 level ) -> Void`

Set minimum severity level. Events below this are dropped.

**Parameters**:

- `level` (`I64`)
- `KernelLogLevel backed value` (`0=Trace through 5=Fatal`)

#### `function getMinLevel( self ) -> I64`

Return current minimum log level. 

#### `function log( self, I64 level, I64 source, I64 code ) -> Boolean`

Log an event at the given severity level with a source identifier and event code. Timestamp is auto-incremented monotonically.

**Parameters**:

- `level` (`I64`)
- `Severity level` (`KernelLogLevel backed value`)
- `source` (`I64`)
- `Numeric identifier of the subsystem producing the event.`
- `code` (`I64`)
- `Application-defined event code.`

**Returns**: `Boolean` — True if event was recorded, False if below min level.

#### `function logTrace( self, I64 source, I64 code ) -> Boolean`

Log a trace-level event. 

#### `function logDebug( self, I64 source, I64 code ) -> Boolean`

Log a debug-level event. 

#### `function logInfo( self, I64 source, I64 code ) -> Boolean`

Log an info-level event. 

#### `function logWarn( self, I64 source, I64 code ) -> Boolean`

Log a warning-level event. 

#### `function logError( self, I64 source, I64 code ) -> Boolean`

Log an error-level event. 

#### `function logFatal( self, I64 source, I64 code ) -> Boolean`

Log a fatal-level event. 

#### `function getCount( self ) -> I64`

Return number of entries currently stored. 

#### `function countAtLevel( self, I64 level ) -> I64`

Count entries at or above the given severity level.

**Parameters**:

- `level` (`I64`)
- `Minimum level to count.`

#### `function getTimestamp( self, I64 index ) -> I64`

Return timestamp of entry at logical index (0 = oldest). 

#### `function getLevel( self, I64 index ) -> I64`

Return severity level of entry at logical index. 

#### `function getSource( self, I64 index ) -> I64`

Return source identifier of entry at logical index. 

#### `function getCode( self, I64 index ) -> I64`

Return event code of entry at logical index. 

#### `function clear( self ) -> Void`

Clear all log entries. 

#### `function destroy( self ) -> Void`

Free all resources held by the event log.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

