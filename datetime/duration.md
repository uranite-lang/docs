# uranite.datetime.duration

## Table of Contents

- [const `NANOS_PER_SEC`](#const-nanos-per-sec)
- [const `NANOS_PER_MILLI`](#const-nanos-per-milli)
- [const `NANOS_PER_MICRO`](#const-nanos-per-micro)
- [const `SECS_PER_MIN`](#const-secs-per-min)
- [const `SECS_PER_HOUR`](#const-secs-per-hour)
- [const `SECS_PER_DAY`](#const-secs-per-day)
- [class `Duration`](#class-duration)
  - [`Duration()`](#Duration)
  - [`totalSeconds()`](#totalSeconds)
  - [`totalMillis()`](#totalMillis)
  - [`totalMicros()`](#totalMicros)
  - [`totalNanos()`](#totalNanos)
  - [`add()`](#add)
  - [`subtract()`](#subtract)
  - [`isZero()`](#isZero)
  - [`isNegative()`](#isNegative)
- [function `fromNanos`](#function-fromnanos)
  - [`fromNanos()`](#fromNanos)
- [function `fromMicros`](#function-frommicros)
  - [`fromMicros()`](#fromMicros)
- [function `fromMillis`](#function-frommillis)
  - [`fromMillis()`](#fromMillis)
- [function `fromSeconds`](#function-fromseconds)
  - [`fromSeconds()`](#fromSeconds)
- [function `fromMinutes`](#function-fromminutes)
  - [`fromMinutes()`](#fromMinutes)
- [function `fromHours`](#function-fromhours)
  - [`fromHours()`](#fromHours)
- [function `fromDays`](#function-fromdays)
  - [`fromDays()`](#fromDays)

## const `NANOS_PER_SEC`

The number of nanoseconds in one second (1,000,000,000). 

## const `NANOS_PER_MILLI`

The number of nanoseconds in one millisecond (1,000,000). 

## const `NANOS_PER_MICRO`

The number of nanoseconds in one microsecond (1,000). 

## const `SECS_PER_MIN`

The number of seconds in one minute (60). 

## const `SECS_PER_HOUR`

The number of seconds in one hour (3,600). 

## const `SECS_PER_DAY`

The number of seconds in one day (86,400). 

## class `Duration`

Represents a span of time decomposed into whole seconds and a nanosecond remainder. The nanosecond component is always normalized to the range [0, 999999999]. Supports arithmetic, comparison, and conversion to various time units.

### Fields

| Name | Type | Access |
|------|------|--------|
| `secs` | `I64` | public |
| `nanos` | `I64` | public |

### Methods

#### `function Duration( self, I64 secs, I64 nanos ) -> Void`

Create a new Duration from seconds and nanoseconds. The nanosecond value is normalized so that it falls within [0, 999999999], carrying any overflow or underflow into the seconds component.

**Parameters**:

- `secs` (`I64`)
- `The whole seconds component.`
- `nanos` (`I64`)
- `The nanosecond component, which will be normalized.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function totalSeconds( self ) -> I64`

Return the total whole seconds in this duration, ignoring the nanosecond remainder.

**Returns**: — The whole seconds component of this duration.

#### `function totalMillis( self ) -> I64`

Return the total duration expressed in whole milliseconds.

**Returns**: — The duration converted to milliseconds, truncating any
sub-millisecond remainder.

#### `function totalMicros( self ) -> I64`

Return the total duration expressed in whole microseconds.

**Returns**: — The duration converted to microseconds, truncating any
sub-microsecond remainder.

#### `function totalNanos( self ) -> I64`

Return the total duration expressed in nanoseconds.

**Returns**: — The entire duration as a single nanosecond count.

#### `function add( self, Duration other ) -> Duration`

Add another Duration to this one and return the result as a new Duration. Neither operand is modified.

**Parameters**:

- `other` (`Duration`)
- `The duration to add.`

**Returns**: — A new Duration representing the sum.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function subtract( self, Duration other ) -> Duration`

Subtract another Duration from this one and return the result as a new Duration. Neither operand is modified.

**Parameters**:

- `other` (`Duration`)
- `The duration to subtract.`

**Returns**: — A new Duration representing the difference.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isZero( self ) -> Boolean`

Check whether this duration represents zero elapsed time.

**Returns**: — True if both seconds and nanoseconds are zero.

#### `function isNegative( self ) -> Boolean`

Check whether this duration is negative.

**Returns**: — True if the seconds component is less than zero.

## function `fromNanos`

Create a Duration from a nanosecond count.

**Parameters**:

- `nanos` (`I64`)
- `The number of nanoseconds.`

**Returns**: — A new Duration equivalent to the given nanosecond count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromNanos( I64 nanos ) -> Duration`

Create a Duration from a nanosecond count.

**Parameters**:

- `nanos` (`I64`)
- `The number of nanoseconds.`

**Returns**: — A new Duration equivalent to the given nanosecond count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromMicros`

Create a Duration from a microsecond count.

**Parameters**:

- `micros` (`I64`)
- `The number of microseconds.`

**Returns**: — A new Duration equivalent to the given microsecond count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromMicros( I64 micros ) -> Duration`

Create a Duration from a microsecond count.

**Parameters**:

- `micros` (`I64`)
- `The number of microseconds.`

**Returns**: — A new Duration equivalent to the given microsecond count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromMillis`

Create a Duration from a millisecond count.

**Parameters**:

- `millis` (`I64`)
- `The number of milliseconds.`

**Returns**: — A new Duration equivalent to the given millisecond count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromMillis( I64 millis ) -> Duration`

Create a Duration from a millisecond count.

**Parameters**:

- `millis` (`I64`)
- `The number of milliseconds.`

**Returns**: — A new Duration equivalent to the given millisecond count.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromSeconds`

Create a Duration from a whole-second count.

**Parameters**:

- `secs` (`I64`)
- `The number of seconds.`

**Returns**: — A new Duration with the given seconds and zero nanoseconds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromSeconds( I64 secs ) -> Duration`

Create a Duration from a whole-second count.

**Parameters**:

- `secs` (`I64`)
- `The number of seconds.`

**Returns**: — A new Duration with the given seconds and zero nanoseconds.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromMinutes`

Create a Duration from a minute count.

**Parameters**:

- `mins` (`I64`)
- `The number of minutes.`

**Returns**: — A new Duration equivalent to the given number of minutes.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromMinutes( I64 mins ) -> Duration`

Create a Duration from a minute count.

**Parameters**:

- `mins` (`I64`)
- `The number of minutes.`

**Returns**: — A new Duration equivalent to the given number of minutes.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromHours`

Create a Duration from an hour count.

**Parameters**:

- `hours` (`I64`)
- `The number of hours.`

**Returns**: — A new Duration equivalent to the given number of hours.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromHours( I64 hours ) -> Duration`

Create a Duration from an hour count.

**Parameters**:

- `hours` (`I64`)
- `The number of hours.`

**Returns**: — A new Duration equivalent to the given number of hours.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fromDays`

Create a Duration from a day count.

**Parameters**:

- `days` (`I64`)
- `The number of days.`

**Returns**: — A new Duration equivalent to the given number of days.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fromDays( I64 days ) -> Duration`

Create a Duration from a day count.

**Parameters**:

- `days` (`I64`)
- `The number of days.`

**Returns**: — A new Duration equivalent to the given number of days.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

