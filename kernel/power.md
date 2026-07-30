# uranite.kernel.power

## Table of Contents

- [Imports](#imports)
- [class `PowerController`](#class-powercontroller)
  - [`PowerController()`](#PowerController)
  - [`requestShutdown()`](#requestShutdown)
  - [`requestReboot()`](#requestReboot)
  - [`suspend()`](#suspend)
  - [`resume()`](#resume)
  - [`haltCpu()`](#haltCpu)
  - [`getSystemState()`](#getSystemState)
  - [`getCpuState()`](#getCpuState)
  - [`registerCallback()`](#registerCallback)
  - [`unregisterCallback()`](#unregisterCallback)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.power.power`
  - `CPU_STATE_ACTIVE`
  - `CPU_STATE_DEEP_SLEEP`
  - `CPU_STATE_HALT`
  - `CPU_STATE_STOP_CLOCK`
  - `POWER_EVENT_REBOOT`
  - `POWER_EVENT_RESUME`
  - `POWER_EVENT_SHUTDOWN`
  - `POWER_EVENT_SUSPEND`
  - `PowerManager`
  - `SYSTEM_OFF`
  - `SYSTEM_RUNNING`
  - `SYSTEM_STANDBY`
  - `SYSTEM_SUSPEND`

## class `PowerController`

System power state management providing suspend, resume, shutdown, and reboot operations with callback registration for power state transitions.

### Fields

| Name | Type | Access |
|------|------|--------|
| `manager` | `PowerManager` | public |

### Methods

#### `function PowerController( self, I64 maxCallbacks ) -> Void`

#### `function requestShutdown( self ) -> Boolean`

Initiate system shutdown. Returns True if shutdown sequence started. 

#### `function requestReboot( self ) -> Void`

Initiate system reboot. Does not return on success. 

#### `function suspend( self ) -> Boolean`

Suspend the system. Returns True when resumed successfully. 

#### `function resume( self ) -> Boolean`

Resume the system from suspended state. 

#### `function haltCpu( self ) -> Void`

Halt the CPU until the next interrupt. 

#### `function getSystemState( self ) -> I64`

Return the current system power state. 

#### `function getCpuState( self ) -> I64`

Return the current CPU power state. 

#### `function registerCallback( self, I64 event, I64 callbackId ) -> I64`

Register a callback for a power event (POWER_EVENT_SUSPEND, POWER_EVENT_RESUME, POWER_EVENT_SHUTDOWN, POWER_EVENT_REBOOT). Returns the callback slot index.

#### `function unregisterCallback( self, I64 slot ) -> Boolean`

Unregister a previously registered power callback. 

#### `function destroy( self ) -> Void`

Release all resources held by the power controller.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

