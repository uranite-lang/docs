# uranite.os.power.power

## Table of Contents

- [Imports](#imports)
- [const `CPU_STATE_ACTIVE`](#const-cpu-state-active)
- [const `CPU_STATE_HALT`](#const-cpu-state-halt)
- [const `CPU_STATE_STOP_CLOCK`](#const-cpu-state-stop-clock)
- [const `CPU_STATE_DEEP_SLEEP`](#const-cpu-state-deep-sleep)
- [const `SYSTEM_RUNNING`](#const-system-running)
- [const `SYSTEM_STANDBY`](#const-system-standby)
- [const `SYSTEM_SUSPEND`](#const-system-suspend)
- [const `SYSTEM_OFF`](#const-system-off)
- [const `POWER_EVENT_SUSPEND`](#const-power-event-suspend)
- [const `POWER_EVENT_RESUME`](#const-power-event-resume)
- [const `POWER_EVENT_SHUTDOWN`](#const-power-event-shutdown)
- [const `POWER_EVENT_REBOOT`](#const-power-event-reboot)
- [class `PowerManager`](#class-powermanager)
  - [`PowerManager()`](#PowerManager)
  - [`registerCallback()`](#registerCallback)
  - [`unregisterCallback()`](#unregisterCallback)
  - [`countCallbacksForEvent()`](#countCallbacksForEvent)
  - [`cpuHalt()`](#cpuHalt)
  - [`suspend()`](#suspend)
  - [`resume()`](#resume)
  - [`shutdown()`](#shutdown)
  - [`reboot()`](#reboot)
  - [`getSystemState()`](#getSystemState)
  - [`getCpuState()`](#getCpuState)
  - [`getCallbackCount()`](#getCallbackCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## const `CPU_STATE_ACTIVE`

CPU C-state for active operation (C0). Fully operational, executing instructions at normal speed. 

## const `CPU_STATE_HALT`

CPU C-state for halt (C1). HLT instruction stops CPU until next interrupt, fast wake-up. 

## const `CPU_STATE_STOP_CLOCK`

CPU C-state for stop-clock (C2). Core clock stopped, deeper savings than C1, slightly higher wake-up latency. 

## const `CPU_STATE_DEEP_SLEEP`

CPU C-state for deep sleep (C3). Caches flushed, deepest idle mode, maximum savings, significant wake-up latency. 

## const `SYSTEM_RUNNING`

System power state for running (S0). Normal operating state, fully powered and active. 

## const `SYSTEM_STANDBY`

System power state for standby (S1). CPU stopped, RAM and peripherals powered, fast resume. 

## const `SYSTEM_SUSPEND`

System power state for suspend-to-RAM (S3). Only RAM powered, CPU and peripherals off. 

## const `SYSTEM_OFF`

System power state for powered-off (S5). Completely shut down except wake-on-LAN circuits. 

## const `POWER_EVENT_SUSPEND`

Power event type for suspend. Broadcast before system enters suspended state. 

## const `POWER_EVENT_RESUME`

Power event type for resume. Broadcast after system resumes from suspended state. 

## const `POWER_EVENT_SHUTDOWN`

Power event type for shutdown. Broadcast before system powers off for cleanup. 

## const `POWER_EVENT_REBOOT`

Power event type for reboot. Broadcast before system performs hardware reset. 

## class `PowerManager`

Manages system and CPU power states, including transitions between active, suspend, and shutdown states. Provides a callback registration mechanism so that kernel subsystems and drivers can be notified of power events (suspend, resume, shutdown, reboot) and perform necessary cleanup or state preservation. The power manager uses a spinlock for thread-safe callback registration and state transitions.

### Fields

| Name | Type | Access |
|------|------|--------|
| `systemState` | `I64` | public |
| `cpuState` | `I64` | public |
| `callbackIds` | `Memory<I64>` | public |
| `callbackEvents` | `Memory<I64>` | public |
| `callbackCapacity` | `I64` | public |
| `callbackCount` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function PowerManager( self, I64 maxCallbacks ) -> Void`

Constructs a new power manager with the specified maximum number of callback registrations. The system starts in the running state (S0) with the CPU in the active state (C0).

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function registerCallback( self, I64 event, I64 callbackId ) -> I64`

Registers a callback to be notified when the specified power event occurs. The callback is identified by its callbackId, which the power event dispatcher uses to invoke the appropriate handler. Returns the slot index on success, or -1 if the callback table is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unregisterCallback( self, I64 slot ) -> Boolean`

Unregisters the callback at the specified slot index, freeing the slot for reuse. Returns True if the callback was successfully unregistered, or False if the slot index is out of bounds or the slot is already empty.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function countCallbacksForEvent( self, I64 event ) -> I64`

Counts and returns the number of callbacks currently registered for the specified power event type. This is useful for determining how many subsystems need to be notified before a power state transition.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function cpuHalt( self ) -> Void`

Enters the CPU halt state (C1) by issuing the x86 HLT instruction. The CPU stops executing instructions and enters a low-power idle state until the next hardware interrupt arrives, at which point execution resumes.

#### `function suspend( self ) -> Boolean`

Requests a system suspend to S3 (suspend-to-RAM). The system must be in the running state (S0) for the transition to proceed. Returns True if the state was successfully changed to suspended, or False if the system is not in the running state.

#### `function resume( self ) -> Boolean`

Resumes the system from the suspended state (S3) back to the running state (S0). The CPU state is also reset to active (C0). Returns True if the resume succeeded, or False if the system was not in the suspended state.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function shutdown( self ) -> Boolean`

Requests a system shutdown, transitioning to the powered-off state (S5). Returns True if the state was successfully changed, or False if the system is already in the powered-off state.

#### `function reboot( self ) -> Void`

Performs a hardware reboot by writing the reset command to the keyboard controller port. This issues an OUT instruction to port 0x64 (100 decimal) with value 0xFE (254 decimal), which triggers a CPU reset on x86 systems.

#### `function getSystemState( self ) -> I64`

Returns the current system power state (S0, S1, S3, or S5). 

#### `function getCpuState( self ) -> I64`

Returns the current CPU C-state (C0, C1, C2, or C3). 

#### `function getCallbackCount( self ) -> I64`

Returns the total number of currently registered power event callbacks. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the power manager, including the callback ID and callback event arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

