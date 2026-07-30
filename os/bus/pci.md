# uranite.os.bus.pci

## Table of Contents

- [Imports](#imports)
- [const `PCI_CONFIG_ADDRESS`](#const-pci-config-address)
- [const `PCI_CONFIG_DATA`](#const-pci-config-data)
- [const `PCI_NO_DEVICE`](#const-pci-no-device)
- [function `pciAddress`](#function-pciaddress)
  - [`pciAddress()`](#pciAddress)
- [function `pciRead32`](#function-pciread32)
  - [`pciRead32()`](#pciRead32)
- [function `pciRead16`](#function-pciread16)
  - [`pciRead16()`](#pciRead16)
- [function `pciRead8`](#function-pciread8)
  - [`pciRead8()`](#pciRead8)
- [function `pciWrite32`](#function-pciwrite32)
  - [`pciWrite32()`](#pciWrite32)
- [class `PciDevice`](#class-pcidevice)
  - [`PciDevice()`](#PciDevice)
  - [`readConfig()`](#readConfig)
  - [`isMultifunction()`](#isMultifunction)
  - [`isBridge()`](#isBridge)
  - [`getBar()`](#getBar)
  - [`isBarMmio()`](#isBarMmio)
  - [`getBarAddress()`](#getBarAddress)
- [class `PciBus`](#class-pcibus)
  - [`PciBus()`](#PciBus)
  - [`scan()`](#scan)
  - [`getCount()`](#getCount)
  - [`getVendorId()`](#getVendorId)
  - [`getDeviceId()`](#getDeviceId)
  - [`getClassCode()`](#getClassCode)
  - [`getSubclass()`](#getSubclass)
  - [`getBus()`](#getBus)
  - [`getDevice()`](#getDevice)
  - [`getFunction()`](#getFunction)
  - [`findByVendorDevice()`](#findByVendorDevice)
  - [`findByClass()`](#findByClass)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.arch.port`
  - `inl`
  - `outl`

## const `PCI_CONFIG_ADDRESS`

PCI configuration address I/O port number (0xCF8). The address port accepts a 32-bit value encoding the bus, device, function, and register offset to select a PCI config register.

## const `PCI_CONFIG_DATA`

PCI configuration data I/O port number (0xCFC). After writing an address to the address port, reading or writing this port accesses the selected PCI configuration register.

## const `PCI_NO_DEVICE`

Vendor ID value indicating no device is present (0xFFFF). When reading vendor ID from an empty PCI slot, the bus returns all ones, which is used as a sentinel to detect absent devices.

## function `pciAddress`

Build a PCI configuration address word for the given bus, device, function, and register offset. The address format is: bit 31 = enable, bits 23:16 = bus, bits 15:11 = device, bits 10:8 = function, bits 7:0 = offset (aligned to 4 bytes).

### Methods

#### `function pciAddress( I64 bus, I64 device, I64 func, I64 offset ) -> I64`

Build a PCI configuration address word for the given bus, device, function, and register offset. The address format is: bit 31 = enable, bits 23:16 = bus, bits 15:11 = device, bits 10:8 = function, bits 7:0 = offset (aligned to 4 bytes).

## function `pciRead32`

Read a 32-bit value from PCI configuration space at the specified bus, device, function, and register offset by writing the config address to port 0xCF8 and reading the result from port 0xCFC.

### Methods

#### `function pciRead32( I64 bus, I64 device, I64 func, I64 offset ) -> I64`

Read a 32-bit value from PCI configuration space at the specified bus, device, function, and register offset by writing the config address to port 0xCF8 and reading the result from port 0xCFC.

## function `pciRead16`

Read a 16-bit value from PCI configuration space by performing a 32-bit read and extracting the appropriate 16-bit half based on the offset alignment.

### Methods

#### `function pciRead16( I64 bus, I64 device, I64 func, I64 offset ) -> I64`

Read a 16-bit value from PCI configuration space by performing a 32-bit read and extracting the appropriate 16-bit half based on the offset alignment.

## function `pciRead8`

Read an 8-bit value from PCI configuration space by performing a 32-bit read and extracting the appropriate byte based on the offset alignment.

### Methods

#### `function pciRead8( I64 bus, I64 device, I64 func, I64 offset ) -> I64`

Read an 8-bit value from PCI configuration space by performing a 32-bit read and extracting the appropriate byte based on the offset alignment.

## function `pciWrite32`

Write a 32-bit value to PCI configuration space at the specified bus, device, function, and register offset by writing the config address to port 0xCF8 and the value to port 0xCFC.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function pciWrite32( I64 bus, I64 device, I64 func, I64 offset, I64 value ) -> Void`

Write a 32-bit value to PCI configuration space at the specified bus, device, function, and register offset by writing the config address to port 0xCF8 and the value to port 0xCFC.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `PciDevice`

Represents a single PCI device function discovered during bus enumeration. Stores the bus/device/function address along with key configuration register values including vendor ID, device ID, class code, subclass, revision, header type, all six Base Address Registers (BARs), and interrupt routing information.

### Fields

| Name | Type | Access |
|------|------|--------|
| `bus` | `I64` | public |
| `device` | `I64` | public |
| `func` | `I64` | public |
| `vendorId` | `I64` | public |
| `deviceId` | `I64` | public |
| `classCode` | `I64` | public |
| `subclass` | `I64` | public |
| `progIf` | `I64` | public |
| `revisionId` | `I64` | public |
| `headerType` | `I64` | public |
| `bar0` | `I64` | public |
| `bar1` | `I64` | public |
| `bar2` | `I64` | public |
| `bar3` | `I64` | public |
| `bar4` | `I64` | public |
| `bar5` | `I64` | public |
| `interruptLine` | `I64` | public |
| `interruptPin` | `I64` | public |

### Methods

#### `function PciDevice( self ) -> Void`

Construct a PCI device with all fields initialized to zero. Call readConfig() to populate the fields from actual hardware registers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readConfig( self, I64 busNumber, I64 deviceNumber, I64 functionNumber ) -> Void`

Read all configuration fields from hardware for the given bus, device, and function. Populates vendor ID, device ID, class code, subclass, programming interface, revision ID, header type, all six BARs, and interrupt line/pin from the PCI configuration space registers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isMultifunction( self ) -> Boolean`

Check if this device is a multifunction device by testing bit 7 of the header type register. Multifunction devices expose up to 8 independent functions on a single device slot.

#### `function isBridge( self ) -> Boolean`

Check if this device is a PCI-to-PCI bridge by testing whether the lower 7 bits of the header type equal 1. Bridge devices connect two PCI buses and require special configuration.

#### `function getBar( self, I64 index ) -> I64`

Get the raw Base Address Register value by index (0 through 5). Returns 0 if the index is out of range. The raw BAR value contains both the base address and type/prefetch flag bits.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isBarMmio( self, I64 index ) -> Boolean`

Check if the Base Address Register at the given index is memory-mapped I/O (MMIO) rather than I/O port space. Bit 0 of the BAR is 0 for MMIO and 1 for I/O space.

#### `function getBarAddress( self, I64 index ) -> I64`

Get the base address from a BAR by masking out the type and flag bits. For MMIO BARs, the lower 4 bits are masked. For I/O space BARs, the lower 2 bits are masked.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `PciBus`

PCI bus scanner that enumerates all devices across all 256 PCI buses, 32 device slots per bus, and up to 8 functions per device. Discovered device information is stored in parallel Memory arrays for vendor ID, device ID, class code, subclass, bus number, device number, and function number, enabling efficient lookup by vendor/device ID or class/subclass.

### Fields

| Name | Type | Access |
|------|------|--------|
| `vendorIds` | `Memory<I64>` | protect |
| `deviceIds` | `Memory<I64>` | protect |
| `classCodes` | `Memory<I64>` | protect |
| `subclasses` | `Memory<I64>` | protect |
| `buses` | `Memory<I64>` | protect |
| `devices` | `Memory<I64>` | protect |
| `functions` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `count` | `I64` | protect |

### Methods

#### `function PciBus( self, I64 maxDevices ) -> Void`

Construct a PCI bus scanner with capacity for the specified maximum number of discovered devices. Allocates parallel arrays to store device metadata for all discovered PCI functions.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function scan( self ) -> Void`

Scan all 256 PCI buses, 32 device slots per bus, and up to 8 functions per multifunction device. For each slot, reads the vendor ID to detect device presence, then checks the header type to determine if the device is multifunction and should have additional functions probed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function addDevice( self, I64 bus, I64 dev, I64 func ) -> Void`

Record a discovered PCI device function by reading its vendor ID, device ID, class code, and subclass from configuration space and storing them in the parallel arrays. Silently discards the device if the scanner has reached its maximum capacity.

#### `function getCount( self ) -> I64`

Return the number of PCI device functions discovered during the last scan. 

#### `function getVendorId( self, I64 index ) -> I64`

Return the vendor ID of the discovered device at the given index, or 0 if out of range. 

#### `function getDeviceId( self, I64 index ) -> I64`

Return the device ID of the discovered device at the given index, or 0 if out of range. 

#### `function getClassCode( self, I64 index ) -> I64`

Return the class code of the discovered device at the given index, or 0 if out of range. 

#### `function getSubclass( self, I64 index ) -> I64`

Return the subclass of the discovered device at the given index, or 0 if out of range. 

#### `function getBus( self, I64 index ) -> I64`

Return the bus number of the discovered device at the given index, or 0 if out of range. 

#### `function getDevice( self, I64 index ) -> I64`

Return the device slot number of the discovered device at the given index, or 0 if out of range. 

#### `function getFunction( self, I64 index ) -> I64`

Return the function number of the discovered device at the given index, or 0 if out of range. 

#### `function findByVendorDevice( self, I64 vendorId, I64 deviceId ) -> I64`

Find the first discovered device matching the given vendor and device ID pair. Returns the index of the matching device, or -1 if no match is found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findByClass( self, I64 classCode, I64 subclass ) -> I64`

Find the first discovered device matching the given class code and subclass pair. Returns the index of the matching device, or -1 if no match is found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Free all Memory arrays used for storing discovered device metadata. Must be called before the PciBus object is deallocated to prevent memory leaks.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

