# uranite.datetime.datetime

## Table of Contents

- [Imports](#imports)
- [const `SYS_CLOCK_GETTIME`](#const-sys-clock-gettime)
- [const `CLOCK_REALTIME`](#const-clock-realtime)
- [const `CLOCK_MONOTONIC`](#const-clock-monotonic)
- [const `EPOCH_YEAR`](#const-epoch-year)
- [const `DAYS_PER_400Y`](#const-days-per-400y)
- [const `DAYS_PER_100Y`](#const-days-per-100y)
- [const `DAYS_PER_4Y`](#const-days-per-4y)
- [function `isLeapYear`](#function-isleapyear)
  - [`isLeapYear()`](#isLeapYear)
- [function `daysInMonth`](#function-daysinmonth)
  - [`daysInMonth()`](#daysInMonth)
- [function `daysInYear`](#function-daysinyear)
  - [`daysInYear()`](#daysInYear)
- [function `clockGettime`](#function-clockgettime)
- [function `currentTimeNanos`](#function-currenttimenanos)
  - [`currentTimeNanos()`](#currentTimeNanos)
- [function `currentTimeMillis`](#function-currenttimemillis)
  - [`currentTimeMillis()`](#currentTimeMillis)
- [class `DateTime`](#class-datetime)
  - [`DateTime()`](#DateTime)
  - [`toTimestamp()`](#toTimestamp)
  - [`dayOfWeek()`](#dayOfWeek)
  - [`dayOfYear()`](#dayOfYear)
  - [`addDuration()`](#addDuration)
  - [`subtractDuration()`](#subtractDuration)
  - [`diffSeconds()`](#diffSeconds)
  - [`isBefore()`](#isBefore)
  - [`isAfter()`](#isAfter)
  - [`isEqual()`](#isEqual)
  - [`format()`](#format)
- [function `padTwo`](#function-padtwo)
  - [`padTwo()`](#padTwo)
- [function `padFour`](#function-padfour)
  - [`padFour()`](#padFour)
- [function `fromTimestamp`](#function-fromtimestamp)
  - [`fromTimestamp()`](#fromTimestamp)
- [function `now`](#function-now)
  - [`now()`](#now)
- [function `nowMonotonic`](#function-nowmonotonic)
  - [`nowMonotonic()`](#nowMonotonic)
- [function `utc`](#function-utc)
  - [`utc()`](#utc)
- [function `date`](#function-date)
  - [`date()`](#date)

## Imports

- `uranite.datetime.duration`
  - `Duration`
  - `SECS_PER_DAY`
- `uranite.datetime.errors`
  - `DateTimeError`
  - `InvalidDateError`
- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `memoryToPtr`
  - `readByteAt`
  - `readI64At`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
  - `writeI64At`
- `uranite.memory.allocator`
  - `alloc`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall2`
- `uranite.os.syscall.result`
  - `SyscallResult`
- `uranite.string.format`
  - `appendTo`
  - `padZero`

## const `SYS_CLOCK_GETTIME`

Linux syscall number for clock_gettime. 

## const `CLOCK_REALTIME`

Clock ID for wall-clock time (affected by NTP adjustments). 

## const `CLOCK_MONOTONIC`

Clock ID for monotonic time (steady, not affected by NTP). 

## const `EPOCH_YEAR`

The Unix epoch year (January 1, 1970). 

## const `DAYS_PER_400Y`

## const `DAYS_PER_100Y`

## const `DAYS_PER_4Y`

## function `isLeapYear`

Determine whether a given year is a leap year according to the Gregorian calendar rules: divisible by 4, except centuries unless also divisible by 400.

**Parameters**:

- `year` (`I64`)
- `The calendar year to check.`

**Returns**: — Boolean:
True if the year is a leap year, False otherwise.

**Complexity**:
- Time: `O(1)`

### Methods

#### `function isLeapYear( I64 year ) -> Boolean`

Determine whether a given year is a leap year according to the Gregorian calendar rules: divisible by 4, except centuries unless also divisible by 400.

**Parameters**:

- `year` (`I64`)
- `The calendar year to check.`

**Returns**: — Boolean:
True if the year is a leap year, False otherwise.

**Complexity**:
- Time: `O(1)`

## function `daysInMonth`

Return the number of days in the given month for the given year. Accounts for leap years when month is February.

**Parameters**:

- `year` (`I64`)
- `The calendar year.`
- `month` (`I64`)
- `The month number` (`1-12`)

**Returns**: — I64:
The number of days in that month, or 0 if month is invalid.

**Complexity**:
- Time: `O(1)`

### Methods

#### `function daysInMonth( I64 year, I64 month ) -> I64`

Return the number of days in the given month for the given year. Accounts for leap years when month is February.

**Parameters**:

- `year` (`I64`)
- `The calendar year.`
- `month` (`I64`)
- `The month number` (`1-12`)

**Returns**: — I64:
The number of days in that month, or 0 if month is invalid.

**Complexity**:
- Time: `O(1)`

## function `daysInYear`

Return the number of days in the given year (365 or 366).

**Parameters**:

- `year` (`I64`)
- `The calendar year.`

**Returns**: — I64:
366 if the year is a leap year, 365 otherwise.

**Complexity**:
- Time: `O(1)`

### Methods

#### `function daysInYear( I64 year ) -> I64`

Return the number of days in the given year (365 or 366).

**Parameters**:

- `year` (`I64`)
- `The calendar year.`

**Returns**: — I64:
366 if the year is a leap year, 365 otherwise.

**Complexity**:
- Time: `O(1)`

## function `clockGettime`

Read the current time from the given kernel clock via the clock_gettime syscall. Returns a Memory buffer containing two I64 values: seconds at offset 0, nanoseconds at offset 8.

**Parameters**:

- `clockId` (`I64`)
- `The clock to read` (`CLOCK_REALTIME or CLOCK_MONOTONIC`)

**Returns**: `Memory<I64>` — A buffer with seconds and nanoseconds.

**Raises**:

- `DateTimeError` → `Error` — If the clock_gettime syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1) for the two-element timespec buffer.`

### Methods

#### `function clockGettime( I64 clockId ) -> Memory<I64>`

Read the current time from the given kernel clock via the clock_gettime syscall. Returns a Memory buffer containing two I64 values: seconds at offset 0, nanoseconds at offset 8.

**Parameters**:

- `clockId` (`I64`)
- `The clock to read` (`CLOCK_REALTIME or CLOCK_MONOTONIC`)

**Returns**: `Memory<I64>` — A buffer with seconds and nanoseconds.

**Raises**:

- `DateTimeError` → `Error` — If the clock_gettime syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1) for the two-element timespec buffer.`

## function `currentTimeNanos`

Returns current wall-clock time as nanoseconds since Unix epoch.

**Returns**: `I64` — Nanoseconds since 1970-01-01 00:00:00 UTC.

**Raises**:

- `DateTimeError` → `Error` — If the clock_gettime syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function currentTimeNanos(  ) -> I64`

Returns current wall-clock time as nanoseconds since Unix epoch.

**Returns**: `I64` — Nanoseconds since 1970-01-01 00:00:00 UTC.

**Raises**:

- `DateTimeError` → `Error` — If the clock_gettime syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `currentTimeMillis`

Returns current wall-clock time as milliseconds since Unix epoch.

**Returns**: `I64` — Milliseconds since 1970-01-01 00:00:00 UTC.

**Raises**:

- `DateTimeError` → `Error` — If the clock_gettime syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function currentTimeMillis(  ) -> I64`

Returns current wall-clock time as milliseconds since Unix epoch.

**Returns**: `I64` — Milliseconds since 1970-01-01 00:00:00 UTC.

**Raises**:

- `DateTimeError` → `Error` — If the clock_gettime syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `DateTime`

Calendar date and time with nanosecond precision and UTC offset.

Represents a point in time decomposed into year, month, day, hour, minute, second, nanosecond, and UTC offset. Supports conversion to/from Unix timestamps, calendar arithmetic via Duration, comparison, and pattern-based string formatting.

### Fields

| Name | Type | Access |
|------|------|--------|
| `year` | `I64` | public |
| `month` | `I64` | public |
| `day` | `I64` | public |
| `hour` | `I64` | public |
| `minute` | `I64` | public |
| `second` | `I64` | public |
| `nanosecond` | `I64` | public |
| `utcOffset` | `I64` | public |

### Methods

#### `function DateTime( self, I64 year, I64 month, I64 day, I64 hour, I64 minute, I64 second, I64 nanosecond, I64 utcOffset ) -> Void`

Create a new DateTime with all components specified.

**Parameters**:

- `year` (`I64`)
- `The calendar year.`
- `month` (`I64`)
- `The month number` (`1-12`)
- `day` (`I64`)
- `The day of the month` (`1-31`)
- `hour` (`I64`)
- `The hour` (`0-23`)
- `minute` (`I64`)
- `The minute` (`0-59`)
- `second` (`I64`)
- `The second` (`0-59`)
- `nanosecond` (`I64`)
- `The nanosecond` (`0-999999999`)
- `utcOffset` (`I64`)
- `The UTC offset in seconds.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toTimestamp( self ) -> I64`

Convert this DateTime to a Unix timestamp (seconds since epoch). The UTC offset is subtracted to produce UTC-relative time.

**Returns**: — I64:
The Unix timestamp in seconds.

**Complexity**:
- Time: `O(y) where y is the number of years since 1970.`

#### `function dayOfWeek( self ) -> I64`

Calculate the day of the week for this DateTime. Sunday is 0, Monday is 1, ..., Saturday is 6.

**Returns**: — I64:
The day of the week (0 = Sunday, 6 = Saturday).

**Complexity**:
- Time: `O(y) where y is the number of years since 1970.`

#### `function dayOfYear( self ) -> I64`

Calculate the day of the year for this DateTime (1-366).

**Returns**: — I64:
The ordinal day within the year.

**Complexity**:
- Time: `O(month)`

#### `function addDuration( self, Duration duration ) -> DateTime`

Return a new DateTime advanced by the given Duration.

**Parameters**:

- `duration` (`Duration`)
- `The duration to add.`

**Returns**: — DateTime:
A new DateTime offset forward by the duration.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function subtractDuration( self, Duration duration ) -> DateTime`

Return a new DateTime moved backward by the given Duration.

**Parameters**:

- `duration` (`Duration`)
- `The duration to subtract.`

**Returns**: — DateTime:
A new DateTime offset backward by the duration.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function diffSeconds( self, DateTime other ) -> I64`

Calculate the difference in seconds between this and another DateTime.

**Parameters**:

- `other` (`DateTime`)
- `The DateTime to compare against.`

**Returns**: `I64` — The number of seconds (self - other). Positive if self is later.

#### `function isBefore( self, DateTime other ) -> Boolean`

Check whether this DateTime is strictly before another.

**Parameters**:

- `other` (`DateTime`)
- `The DateTime to compare against.`

**Returns**: `Boolean` — True if this DateTime is earlier than other.

#### `function isAfter( self, DateTime other ) -> Boolean`

Check whether this DateTime is strictly after another.

**Parameters**:

- `other` (`DateTime`)
- `The DateTime to compare against.`

**Returns**: `Boolean` — True if this DateTime is later than other.

#### `function isEqual( self, DateTime other ) -> Boolean`

Check whether this DateTime represents the same instant as another.

**Parameters**:

- `other` (`DateTime`)
- `The DateTime to compare against.`

**Returns**: `Boolean` — True if both represent the same Unix timestamp.

#### `function format( self, String pattern ) -> String`

Format this DateTime using a simple pattern string. Supported tokens: YYYY (4-digit year), MM (2-digit month), DD (2-digit day), hh (2-digit hour), mm (2-digit minute), ss (2-digit second). All other characters pass through unchanged.

**Parameters**:

- `pattern` (`String`)
- `The format pattern string.`

**Returns**: — String:
The formatted date/time string.

**Complexity**:
- Time: `O(n) where n is the byte length of pattern.`
- Space: `O(n) for the output buffer.`

## function `padTwo`

Format an integer as a 2-digit zero-padded string. Kept for backward compatibility — prefer padZero(value, 2).

**Parameters**:

- `value` (`I64`)
- `The integer value to format.`

**Returns**: `String` — A 2-character zero-padded string.

### Methods

#### `function padTwo( I64 value ) -> String`

Format an integer as a 2-digit zero-padded string. Kept for backward compatibility — prefer padZero(value, 2).

**Parameters**:

- `value` (`I64`)
- `The integer value to format.`

**Returns**: `String` — A 2-character zero-padded string.

## function `padFour`

Format an integer as a 4-digit zero-padded string. Kept for backward compatibility — prefer padZero(value, 4).

**Parameters**:

- `value` (`I64`)
- `The integer value to format.`

**Returns**: `String` — A 4-character zero-padded string.

### Methods

#### `function padFour( I64 value ) -> String`

Format an integer as a 4-digit zero-padded string. Kept for backward compatibility — prefer padZero(value, 4).

**Parameters**:

- `value` (`I64`)
- `The integer value to format.`

**Returns**: `String` — A 4-character zero-padded string.

## function `fromTimestamp`

Create a DateTime from a Unix timestamp, nanoseconds, and UTC offset. Decomposes the timestamp into year, month, day, hour, minute, second using iterative subtraction over years and months.

**Parameters**:

- `timestamp` (`I64`)
- `The Unix timestamp in seconds since epoch.`
- `nanos` (`I64`)
- `The nanosecond component` (`0-999999999`)
- `utcOffset` (`I64`)
- `The UTC offset in seconds to apply.`

**Returns**: — DateTime:
A new DateTime decomposed from the given timestamp.

**Complexity**:
- Time: `O(y) where y is the number of years since 1970.`

### Methods

#### `function fromTimestamp( I64 timestamp, I64 nanos, I64 utcOffset ) -> DateTime`

Create a DateTime from a Unix timestamp, nanoseconds, and UTC offset. Decomposes the timestamp into year, month, day, hour, minute, second using iterative subtraction over years and months.

**Parameters**:

- `timestamp` (`I64`)
- `The Unix timestamp in seconds since epoch.`
- `nanos` (`I64`)
- `The nanosecond component` (`0-999999999`)
- `utcOffset` (`I64`)
- `The UTC offset in seconds to apply.`

**Returns**: — DateTime:
A new DateTime decomposed from the given timestamp.

**Complexity**:
- Time: `O(y) where y is the number of years since 1970.`

## function `now`

Get the current wall-clock time as a DateTime with UTC offset 0.

**Returns**: — DateTime:
The current time from CLOCK_REALTIME.

**Complexity**:
- Time: `O(y) where y is the number of years since 1970.`

### Methods

#### `function now(  ) -> DateTime`

Get the current wall-clock time as a DateTime with UTC offset 0.

**Returns**: — DateTime:
The current time from CLOCK_REALTIME.

**Complexity**:
- Time: `O(y) where y is the number of years since 1970.`

## function `nowMonotonic`

Get the current monotonic time as nanoseconds since an arbitrary fixed point. Suitable for measuring elapsed time, not wall-clock.

**Returns**: — I64:
Monotonic time in nanoseconds.

**Complexity**:
- Time: `O(1)`

### Methods

#### `function nowMonotonic(  ) -> I64`

Get the current monotonic time as nanoseconds since an arbitrary fixed point. Suitable for measuring elapsed time, not wall-clock.

**Returns**: — I64:
Monotonic time in nanoseconds.

**Complexity**:
- Time: `O(1)`

## function `utc`

Create a DateTime at UTC (offset 0) with the given components.

**Parameters**:

- `year` (`I64`)
- `The calendar year.`
- `month` (`I64`)
- `The month` (`1-12`)
- `day` (`I64`)
- `The day` (`1-31`)
- `hour` (`I64`)
- `The hour` (`0-23`)
- `minute` (`I64`)
- `The minute` (`0-59`)
- `second` (`I64`)
- `The second` (`0-59`)

**Returns**: — DateTime:
A new DateTime at UTC with nanosecond 0.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function utc( I64 year, I64 month, I64 day, I64 hour, I64 minute, I64 second ) -> DateTime`

Create a DateTime at UTC (offset 0) with the given components.

**Parameters**:

- `year` (`I64`)
- `The calendar year.`
- `month` (`I64`)
- `The month` (`1-12`)
- `day` (`I64`)
- `The day` (`1-31`)
- `hour` (`I64`)
- `The hour` (`0-23`)
- `minute` (`I64`)
- `The minute` (`0-59`)
- `second` (`I64`)
- `The second` (`0-59`)

**Returns**: — DateTime:
A new DateTime at UTC with nanosecond 0.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `date`

Create a date-only DateTime at midnight UTC with the given year, month, and day.

**Parameters**:

- `year` (`I64`)
- `The calendar year.`
- `month` (`I64`)
- `The month` (`1-12`)
- `day` (`I64`)
- `The day of the month` (`1-31`)

**Returns**: — DateTime:
A new DateTime at midnight UTC.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function date( I64 year, I64 month, I64 day ) -> DateTime`

Create a date-only DateTime at midnight UTC with the given year, month, and day.

**Parameters**:

- `year` (`I64`)
- `The calendar year.`
- `month` (`I64`)
- `The month` (`1-12`)
- `day` (`I64`)
- `The day of the month` (`1-31`)

**Returns**: — DateTime:
A new DateTime at midnight UTC.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

