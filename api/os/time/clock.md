# uranite.os.time.clock

## Table of Contents

- [Imports](#imports)
- [class `SystemClock`](#class-systemclock)
  - [`SystemClock()`](#SystemClock)
  - [`tick()`](#tick)
  - [`advance()`](#advance)
  - [`getTicks()`](#getTicks)
  - [`getSeconds()`](#getSeconds)
  - [`getNanoseconds()`](#getNanoseconds)
  - [`getFrequency()`](#getFrequency)
  - [`uptimeMillis()`](#uptimeMillis)
  - [`uptimeMicros()`](#uptimeMicros)

## Imports

- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `SystemClock`

Monotonic system clock driven by a hardware timer interrupt. Tracks both raw tick counts and derived wall-clock time in seconds and nanoseconds since boot. The tick frequency determines the timer resolution (e.g., 1000 for 1ms ticks, 1000000 for 1us ticks). Nanoseconds per tick is precomputed at construction to avoid division in the interrupt handler hot path. A spinlock protects concurrent access from interrupt handlers and kernel threads.

### Fields

| Name | Type | Access |
|------|------|--------|
| `ticks` | `I64` | public |
| `seconds` | `I64` | public |
| `nanoseconds` | `I64` | public |
| `tickFrequency` | `I64` | public |
| `nanosPerTick` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function SystemClock( self, I64 frequency ) -> Void`

Constructs a new system clock with the specified tick frequency (ticks per second). The nanoseconds-per-tick value is precomputed as 1000000000 divided by the frequency for efficient time tracking in the interrupt handler.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function tick( self ) -> Void`

Advances the clock by one tick. Called by the timer interrupt handler on each hardware timer tick. Increments the raw tick counter, adds the precomputed nanoseconds-per-tick to the nanosecond accumulator, and rolls over into the seconds counter when nanoseconds reach one billion.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function advance( self, I64 count ) -> Void`

Advances the clock by multiple ticks at once. This is used for catch-up after the system resumes from a sleep state where timer interrupts may have been missed. The total nanosecond increment is calculated in bulk and rolled over into seconds as needed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getTicks( self ) -> I64`

Returns the total number of timer ticks since the clock was initialized. 

#### `function getSeconds( self ) -> I64`

Returns the whole seconds elapsed since the clock was initialized. 

#### `function getNanoseconds( self ) -> I64`

Returns the sub-second nanosecond component of the current clock time. 

#### `function getFrequency( self ) -> I64`

Returns the tick frequency (ticks per second) configured for this clock. 

#### `function uptimeMillis( self ) -> I64`

Calculates and returns the total elapsed milliseconds since boot, computed from the seconds and nanoseconds fields.

#### `function uptimeMicros( self ) -> I64`

Calculates and returns the total elapsed microseconds since boot, computed from the seconds and nanoseconds fields.

