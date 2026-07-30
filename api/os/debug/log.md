# uranite.os.debug.log

## Table of Contents

- [Imports](#imports)
- [enum `KernelLogLevel`](#enum-kernelloglevel)
- [class `EventLog`](#class-eventlog)
  - [`EventLog()`](#EventLog)
  - [`log()`](#log)
  - [`getTimestamp()`](#getTimestamp)
  - [`getLevel()`](#getLevel)
  - [`getSource()`](#getSource)
  - [`getCode()`](#getCode)
  - [`setMinLevel()`](#setMinLevel)
  - [`getMinLevel()`](#getMinLevel)
  - [`getCount()`](#getCount)
  - [`countAtLevel()`](#countAtLevel)
  - [`clear()`](#clear)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## enum `KernelLogLevel`

Kernel log severity levels, from most verbose (Trace) to most severe (Fatal). Used by EventLog to filter and categorize kernel events.

## class `EventLog`

Ring buffer-based kernel event log that stores timestamped log entries with severity levels. When the buffer is full, new entries overwrite the oldest ones. Each entry consists of a timestamp, log level, source identifier, and event code stored in parallel Memory arrays. A spinlock protects concurrent access from multiple kernel subsystems and interrupt handlers.

### Fields

| Name | Type | Access |
|------|------|--------|
| `timestamps` | `Memory<I64>` | public |
| `levels` | `Memory<I64>` | public |
| `sources` | `Memory<I64>` | public |
| `codes` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `head` | `I64` | public |
| `tail` | `I64` | public |
| `count` | `I64` | public |
| `minLevel` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function EventLog( self, I64 maxEntries ) -> Void`

Construct an event log with capacity for the specified maximum number of entries. Allocates parallel arrays for timestamps, levels, sources, and codes, initializing all slots to zero. The minimum log level filter defaults to 0 (trace), accepting all messages.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function log( self, I64 timestamp, I64 level, I64 source, I64 code ) -> Boolean`

Log an event with the given timestamp, severity level, source identifier, and event code. Events below the minimum log level are silently dropped and return False. When the ring buffer is full, the oldest entry is overwritten by advancing the head pointer. Returns True if the event was recorded successfully.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getTimestamp( self, I64 index ) -> I64`

Return the timestamp of the log entry at the given logical index, where index 0 is the oldest entry. Returns 0 if the index is out of range.

#### `function getLevel( self, I64 index ) -> I64`

Return the severity level of the log entry at the given logical index, where index 0 is the oldest entry. Returns 0 if the index is out of range.

#### `function getSource( self, I64 index ) -> I64`

Return the source identifier of the log entry at the given logical index, where index 0 is the oldest entry. Returns 0 if the index is out of range.

#### `function getCode( self, I64 index ) -> I64`

Return the event code of the log entry at the given logical index, where index 0 is the oldest entry. Returns 0 if the index is out of range.

#### `function setMinLevel( self, I64 level ) -> Void`

Set the minimum log level filter. Events with a severity level below this threshold will be silently dropped by the log() method.

#### `function getMinLevel( self ) -> I64`

Return the current minimum log level filter threshold. 

#### `function getCount( self ) -> I64`

Return the number of log entries currently stored in the ring buffer. 

#### `function countAtLevel( self, I64 level ) -> I64`

Count the number of log entries at or above the specified severity level by scanning all stored entries. This is a linear-time operation over all entries in the ring buffer.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function clear( self ) -> Void`

Clear all log entries by resetting the head, tail, and count to zero. The underlying memory is not zeroed for performance; old data may remain in the buffer but is inaccessible through the public interface.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Free all Memory arrays used for storing log entries. Must be called before the EventLog object is deallocated to prevent memory leaks.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

