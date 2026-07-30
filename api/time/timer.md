# uranite.time.timer

## Table of Contents

- [Imports](#imports)
- [class `Timer`](#class-timer)
  - [`Timer()`](#Timer)
  - [`scheduleOnce()`](#scheduleOnce)
  - [`schedulePeriodic()`](#schedulePeriodic)
  - [`cancel()`](#cancel)
  - [`tick()`](#tick)
  - [`advanceTicks()`](#advanceTicks)
  - [`drain()`](#drain)
  - [`getTimerState()`](#getTimerState)
  - [`getTimerCallback()`](#getTimerCallback)
  - [`getActiveCount()`](#getActiveCount)
  - [`getUptimeMillis()`](#getUptimeMillis)
  - [`getUptimeMicros()`](#getUptimeMicros)
  - [`getCurrentTick()`](#getCurrentTick)
  - [`destroy()`](#destroy)
- [const `SYS_NANOSLEEP`](#const-sys-nanosleep)
- [function `sleep`](#function-sleep)
  - [`sleep()`](#sleep)
- [function `sleepSeconds`](#function-sleepseconds)
  - [`sleepSeconds()`](#sleepSeconds)

## Imports

- `uranite.io.syscall`
  - `memoryToPtr`
  - `writeI64At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall2`
- `uranite.os.syscall.result`
  - `SyscallResult`
- `uranite.os.time.clock`
  - `SystemClock`
- `uranite.os.time.timer`
  - `TIMER_ACTIVE`
  - `TIMER_EXPIRED`
  - `TIMER_INACTIVE`
  - `TIMER_ONE_SHOT`
  - `TIMER_PERIODIC`
  - `TimerEntry`
  - `TimerWheel`

## class `Timer`

High-level timer with one-shot and periodic scheduling. Combines a kernel TimerWheel with a SystemClock to compute absolute deadlines from relative delay values. Drives expiration via tick advancement.

### Fields

| Name | Type | Access |
|------|------|--------|
| `wheel` | `TimerWheel` | protect |
| `clock` | `SystemClock` | protect |

### Methods

#### `function Timer( self, I64 maxTimers, I64 tickFrequency ) -> Void`

Construct a timer with the given capacity and clock frequency.

**Parameters**:

- `maxTimers` (`I64`)
- `Maximum number of concurrent timer slots.`
- `tickFrequency` (`I64`)
- `Clock ticks per second` (`e.g., 1000 for millisecond resolution`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function scheduleOnce( self, I64 callbackId, I64 delayTicks ) -> I64`

Schedule a one-shot timer that fires after delayTicks from now. Returns timer ID on success, -1 if wheel is full.

**Parameters**:

- `callbackId` (`I64`)
- `Application-defined callback identifier dispatched on fire.`
- `delayTicks` (`I64`)
- `Number of ticks from now until the timer fires.`

**Returns**: `I64` — Timer ID on success, -1 if no slots available.

#### `function schedulePeriodic( self, I64 callbackId, I64 intervalTicks ) -> I64`

Schedule a periodic timer that first fires after intervalTicks from now, then repeats at the same interval.

**Parameters**:

- `callbackId` (`I64`)
- `Application-defined callback identifier dispatched on fire.`
- `intervalTicks` (`I64`)
- `Interval in ticks between each firing.`

**Returns**: `I64` — Timer ID on success, -1 if no slots available.

#### `function cancel( self, I64 slot ) -> Boolean`

Cancel an active timer at the given slot. Returns True on success.

**Parameters**:

- `slot` (`I64`)
- `Timer slot index to cancel.`

#### `function tick( self ) -> I64`

Advance the clock by one tick and fire any expired timers. Returns the number of timers that fired.

#### `function advanceTicks( self, I64 count ) -> I64`

Advance the clock by multiple ticks (catch-up after sleep) and fire any expired timers.

**Parameters**:

- `count` (`I64`)
- `Number of ticks to advance.`

**Returns**: `I64` — Number of timers that fired.

#### `function drain( self ) -> I64`

Reclaim all expired one-shot timer slots for reuse. Returns the number of slots drained.

#### `function getTimerState( self, I64 slot ) -> I64`

Return state of timer at slot (0=inactive, 1=active, 2=expired).

**Parameters**:

- `slot` (`I64`)
- `Timer slot index.`

#### `function getTimerCallback( self, I64 slot ) -> I64`

Return callback ID associated with timer at slot.

**Parameters**:

- `slot` (`I64`)
- `Timer slot index.`

#### `function getActiveCount( self ) -> I64`

Return number of active and expired timers in the wheel. 

#### `function getUptimeMillis( self ) -> I64`

Return total elapsed milliseconds since timer construction. 

#### `function getUptimeMicros( self ) -> I64`

Return total elapsed microseconds since timer construction. 

#### `function getCurrentTick( self ) -> I64`

Return raw tick count since timer construction. 

#### `function destroy( self ) -> Void`

Release all resources held by the timer wheel. 

## const `SYS_NANOSLEEP`

## function `sleep`

Suspend the current thread for the specified duration.

Uses the nanosleep syscall. The actual sleep may be slightly longer due to scheduling granularity.

**Parameters**:

- `milliseconds` (`I64`)
- `The number of milliseconds to sleep.`

**Complexity**:
- Time: `O(1) (blocks for the specified duration)`
- Space: `O(1)`

### Methods

#### `function sleep( I64 milliseconds ) -> Void`

Suspend the current thread for the specified duration.

Uses the nanosleep syscall. The actual sleep may be slightly longer due to scheduling granularity.

**Parameters**:

- `milliseconds` (`I64`)
- `The number of milliseconds to sleep.`

**Complexity**:
- Time: `O(1) (blocks for the specified duration)`
- Space: `O(1)`

## function `sleepSeconds`

Suspend the current thread for the specified number of seconds.

**Parameters**:

- `seconds` (`I64`)
- `The number of seconds to sleep.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function sleepSeconds( I64 seconds ) -> Void`

Suspend the current thread for the specified number of seconds.

**Parameters**:

- `seconds` (`I64`)
- `The number of seconds to sleep.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

