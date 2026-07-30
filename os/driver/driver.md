# uranite.os.driver.driver

## Table of Contents

- [Imports](#imports)
- [enum `DriverState`](#enum-driverstate)
- [interface `Driver`](#interface-driver)
  - [`probe()`](#probe)
  - [`init()`](#init)
  - [`attach()`](#attach)
  - [`detach()`](#detach)
  - [`getDriverId()`](#getDriverId)
  - [`getState()`](#getState)
- [class `DriverDescriptor`](#class-driverdescriptor)
  - [`DriverDescriptor()`](#DriverDescriptor)
  - [`matches()`](#matches)
  - [`isBound()`](#isBound)
  - [`isAvailable()`](#isAvailable)
- [class `DriverRegistry`](#class-driverregistry)
  - [`DriverRegistry()`](#DriverRegistry)
  - [`registerDriver()`](#registerDriver)
  - [`unregisterDriver()`](#unregisterDriver)
  - [`findDriverForDevice()`](#findDriverForDevice)
  - [`bindDriver()`](#bindDriver)
  - [`unbindDriver()`](#unbindDriver)
  - [`getDriverState()`](#getDriverState)
  - [`setDriverState()`](#setDriverState)
  - [`getCount()`](#getCount)
  - [`getDriverId()`](#getDriverId)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## enum `DriverState`

Represents the lifecycle state of a device driver. Drivers transition through these states during their lifecycle: Unloaded -> Probing -> Initialized -> Attached -> Detached. The Failed state indicates that the driver encountered an error during initialization or operation.

## interface `Driver`

Device driver interface defining the contract that all device drivers must implement. The lifecycle follows probe -> init -> attach -> detach, where probe checks hardware compatibility, init allocates resources, attach begins operation, and detach stops operation and releases resources.

### Methods

#### `function probe( self, I64 vendorId, I64 deviceId, I64 classCode ) -> Boolean`

Check if this driver can handle the device with the given vendor ID, device ID, and class code. 

#### `function init( self ) -> Boolean`

Initialize driver resources such as allocating buffers, mapping MMIO regions, and configuring hardware registers. 

#### `function attach( self ) -> Boolean`

Attach to the device and start operation, enabling interrupt handling and data transfer. 

#### `function detach( self ) -> Void`

Detach from the device, stop all operation, and release all allocated resources. 

#### `function getDriverId( self ) -> I64`

Return the unique integer identifier for this driver. 

#### `function getState( self ) -> I64`

Return the current lifecycle state of this driver as an integer matching DriverState values.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `DriverDescriptor`

Metadata descriptor for a registered device driver. Stores the driver's match criteria (vendor ID, device ID, class code) used to determine which hardware devices this driver can handle, along with the current lifecycle state and the bus/slot/function of any bound device.

### Fields

| Name | Type | Access |
|------|------|--------|
| `driverId` | `I64` | public |
| `vendorMatch` | `I64` | public |
| `deviceMatch` | `I64` | public |
| `classMatch` | `I64` | public |
| `state` | `I64` | public |
| `boundDeviceBus` | `I64` | public |
| `boundDeviceSlot` | `I64` | public |
| `boundDeviceFunction` | `I64` | public |

### Methods

#### `function DriverDescriptor( self ) -> Void`

Construct a driver descriptor with all fields initialized to zero, representing an unregistered driver with no match criteria and no bound device.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function matches( self, I64 vendorId, I64 deviceId, I64 classCode ) -> Boolean`

Check if this driver descriptor matches the given device identifiers. A match criterion of 0 acts as a wildcard that matches any value. All three criteria (vendor, device, class) must match for the descriptor to be considered compatible with the device.

#### `function isBound( self ) -> Boolean`

Return whether this driver is currently bound to a device (state Attached = 3). 

#### `function isAvailable( self ) -> Boolean`

Return whether this driver is available for binding (state Unloaded = 0 or Detached = 5).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `DriverRegistry`

Registry that tracks all registered device drivers and their lifecycle states. Provides methods to register, unregister, find, bind, and unbind drivers to hardware devices. Driver metadata is stored in parallel Memory arrays indexed by slot position. Match criteria of 0 act as wildcards.

### Fields

| Name | Type | Access |
|------|------|--------|
| `driverIds` | `Memory<I64>` | protect |
| `vendorMatches` | `Memory<I64>` | protect |
| `deviceMatches` | `Memory<I64>` | protect |
| `classMatches` | `Memory<I64>` | protect |
| `states` | `Memory<I64>` | protect |
| `boundBuses` | `Memory<I64>` | protect |
| `boundSlots` | `Memory<I64>` | protect |
| `boundFunctions` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `count` | `I64` | protect |

### Methods

#### `function DriverRegistry( self, I64 maxDrivers ) -> Void`

Construct a driver registry with capacity for the specified maximum number of drivers. Allocates parallel arrays for driver IDs, match criteria, states, and bound device information, initializing all slots to zero.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function registerDriver( self, I64 driverId, I64 vendorMatch, I64 deviceMatch, I64 classMatch ) -> Boolean`

Register a driver with the given ID and match criteria. A match value of 0 for vendorMatch, deviceMatch, or classMatch acts as a wildcard that matches any device. The driver is placed in the first available slot. Returns True if registration succeeded, False if the registry is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unregisterDriver( self, I64 driverId ) -> Boolean`

Unregister a driver by its ID, clearing all associated metadata and freeing its slot for reuse. Returns True if the driver was found and removed, False if no driver with the given ID exists.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findDriverForDevice( self, I64 vendorId, I64 deviceId, I64 classCode ) -> I64`

Find the first registered driver whose match criteria are compatible with the given device identifiers. Returns the driver ID on match, or -1 if no compatible driver is registered.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function bindDriver( self, I64 driverId, I64 bus, I64 slot, I64 func ) -> Boolean`

Bind the driver with the given ID to a specific PCI device identified by bus, slot, and function. Sets the driver state to Attached (3). Returns True if the driver was found and bound, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unbindDriver( self, I64 driverId ) -> Boolean`

Unbind the driver with the given ID from its currently bound device. Sets the driver state to Detached (5) and clears the bound device information. Returns True if the driver was found, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getDriverState( self, I64 driverId ) -> I64`

Return the lifecycle state of the driver with the given ID. Returns -1 if no driver with the given ID is registered.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function setDriverState( self, I64 driverId, I64 state ) -> Boolean`

Set the lifecycle state of the driver with the given ID. Returns True if the driver was found and its state updated, False if no driver with the given ID is registered.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Return the number of drivers currently registered in the registry. 

#### `function getDriverId( self, I64 index ) -> I64`

Return the driver ID at the given registry slot index, or 0 if the index is out of range. 

#### `function destroy( self ) -> Void`

Free all Memory arrays used for storing driver registry data.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

