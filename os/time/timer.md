# uranite.os.time.timer

## Table of Contents

- [Imports](#imports)
- [const `TIMER_ONE_SHOT`](#const-timer-one-shot)
- [const `TIMER_PERIODIC`](#const-timer-periodic)
- [const `TIMER_INACTIVE`](#const-timer-inactive)
- [const `TIMER_ACTIVE`](#const-timer-active)
- [const `TIMER_EXPIRED`](#const-timer-expired)
- [struct `TimerEntry`](#struct-timerentry)
  - [`TimerEntry()`](#TimerEntry)
- [class `TimerWheel`](#class-timerwheel)
  - [`TimerWheel()`](#TimerWheel)
  - [`scheduleOneShot()`](#scheduleOneShot)
  - [`schedulePeriodic()`](#schedulePeriodic)
  - [`fire()`](#fire)
  - [`cancel()`](#cancel)
  - [`getCallbackId()`](#getCallbackId)
  - [`getState()`](#getState)
  - [`drain()`](#drain)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## const `TIMER_ONE_SHOT`

Timer mode for one-shot. Fires once at deadline tick, then transitions to expired. 

## const `TIMER_PERIODIC`

Timer mode for periodic. Reschedules by adding interval after each fire. 

## const `TIMER_INACTIVE`

Timer state for inactive. Slot available for reuse, skipped during expiration checks. 

## const `TIMER_ACTIVE`

Timer state for active. Has valid deadline, fires when tick reaches or exceeds it. 

## const `TIMER_EXPIRED`

Timer state for expired one-shot. Fired and awaiting drain (slot reclaim). 

## struct `TimerEntry`

Represents a single timer entry with its identifier, deadline tick, repeat interval, operating mode (one-shot or periodic), current state (inactive, active, or expired), and associated callback identifier for dispatch when the timer fires.

### Fields

| Name | Type | Access |
|------|------|--------|
| `id` | `I64` | public |
| `deadline` | `I64` | public |
| `interval` | `I64` | public |
| `mode` | `I64` | public |
| `state` | `I64` | public |
| `callbackId` | `I64` | public |

### Methods

#### `function TimerEntry( self ) -> Void`

Constructs a new timer entry with all fields initialized to zero, representing an inactive timer slot.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `TimerWheel`

Timer wheel data structure that manages one-shot and periodic software timers. Each slot holds a timer with a deadline tick, interval (for periodic timers), mode, state, and callback identifier. The scheduler or interrupt handler calls fire() on each tick to check for and process expired timers. One-shot timers transition to the expired state upon firing, while periodic timers automatically reschedule by advancing their deadline. A spinlock protects concurrent access during timer scheduling and cancellation.

### Fields

| Name | Type | Access |
|------|------|--------|
| `deadlines` | `Memory<I64>` | public |
| `intervals` | `Memory<I64>` | public |
| `modes` | `Memory<I64>` | public |
| `states` | `Memory<I64>` | public |
| `callbackIds` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `count` | `I64` | public |
| `nextId` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function TimerWheel( self, I64 maxTimers ) -> Void`

Constructs a new timer wheel with the specified maximum number of timer slots. All slots are initialized to the inactive state with zeroed fields. The next timer ID starts at 1.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function scheduleOneShot( self, I64 deadline, I64 callbackId ) -> I64`

Schedules a one-shot timer that fires once when the system tick count reaches the specified deadline. Returns the timer ID on success, or -1 if the timer wheel is full.

#### `function schedulePeriodic( self, I64 deadline, I64 interval, I64 callbackId ) -> I64`

Schedules a periodic timer that first fires at the specified deadline tick and then repeats at the given interval. Returns the timer ID on success, or -1 if the timer wheel is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function schedule( self, I64 deadline, I64 interval, I64 mode, I64 callbackId ) -> I64`

Internal method that allocates a timer slot and configures it with the specified deadline, interval, mode (one-shot or periodic), and callback identifier. Returns the timer ID on success, or -1 if no free slots are available.

#### `function fire( self, I64 currentTick ) -> I64`

Checks all active timers against the current tick count and fires any whose deadline has been reached. Periodic timers are automatically rescheduled by advancing their deadline by their interval. One-shot timers transition to the expired state (2). Returns the number of timers that fired during this call.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function cancel( self, I64 slot ) -> Boolean`

Cancels an active timer at the specified slot index, setting its state to inactive and freeing the slot for reuse. Returns True if the timer was successfully cancelled, or False if the slot is out of bounds or already inactive.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCallbackId( self, I64 slot ) -> I64`

Returns the callback identifier associated with the timer at the specified slot index. Returns 0 if the slot is out of bounds.

#### `function getState( self, I64 slot ) -> I64`

Returns the current state of the timer at the specified slot index (0 = inactive, 1 = active, 2 = expired). Returns 0 if the slot is out of bounds.

#### `function drain( self ) -> I64`

Reclaims all expired one-shot timer slots by setting their state back to inactive, freeing them for reuse. Returns the number of timer slots that were drained.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Returns the number of active and expired timers currently in the timer wheel. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the timer wheel, including the deadlines, intervals, modes, states, and callback ID arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

