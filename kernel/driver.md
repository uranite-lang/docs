# uranite.kernel.driver

## Table of Contents

- [Imports](#imports)
- [class `DeviceDriverManager`](#class-devicedrivermanager)
  - [`DeviceDriverManager()`](#DeviceDriverManager)
  - [`registerDriver()`](#registerDriver)
  - [`findDriverForDevice()`](#findDriverForDevice)
  - [`bindDriver()`](#bindDriver)
  - [`getDriverState()`](#getDriverState)
  - [`getDriverCount()`](#getDriverCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.driver.driver`
  - `Driver`
  - `DriverDescriptor`
  - `DriverRegistry`
  - `DriverState`

## class `DeviceDriverManager`

Manages the full driver lifecycle: registration, probing, and state tracking. Wraps DriverRegistry with a higher-level management API.

### Fields

| Name | Type | Access |
|------|------|--------|
| `registry` | `DriverRegistry` | public |

### Methods

#### `function DeviceDriverManager( self, I64 capacity ) -> Void`

Construct a driver manager that can hold up to capacity driver registrations.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function registerDriver( self, I64 driverId, I64 vendorMatch, I64 deviceMatch, I64 classMatch ) -> Boolean`

Register a driver with matching criteria. Returns True on success.

**Parameters**:

- `driverId` (`I64`)
- `Unique driver identifier.`
- `vendorMatch` (`I64`)
- `Vendor ID to match against devices.`
- `deviceMatch` (`I64`)
- `Device ID to match against devices.`
- `classMatch` (`I64`)
- `Device class code to match.`

#### `function findDriverForDevice( self, I64 vendorId, I64 deviceId, I64 classCode ) -> I64`

Find a registered driver matching the given device identifiers. Returns driver ID or -1 if no match.

#### `function bindDriver( self, I64 driverId, I64 bus, I64 slot, I64 func ) -> Boolean`

Bind a driver to a specific PCI device location.

#### `function getDriverState( self, I64 driverId ) -> I64`

Return the current state of a registered driver. 

#### `function getDriverCount( self ) -> I64`

Return the number of registered drivers. 

#### `function destroy( self ) -> Void`

Release all resources held by the driver manager.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

