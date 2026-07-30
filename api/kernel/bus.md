# uranite.kernel.bus

## Table of Contents

- [Imports](#imports)
- [class `PciScanner`](#class-pciscanner)
  - [`PciScanner()`](#PciScanner)
  - [`scan()`](#scan)
  - [`getDeviceCount()`](#getDeviceCount)
  - [`findByVendorDevice()`](#findByVendorDevice)
  - [`findByClass()`](#findByClass)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.bus.pci`
  - `PCI_CONFIG_ADDRESS`
  - `PCI_CONFIG_DATA`
  - `PCI_NO_DEVICE`
  - `PciBus`
  - `PciDevice`
  - `pciAddress`
  - `pciRead16`
  - `pciRead32`
  - `pciRead8`
  - `pciWrite32`

## class `PciScanner`

High-level PCI bus scanner with device enumeration. Wraps the raw PCI configuration space access functions and PciBus device list into a single scan-and-query interface.

### Fields

| Name | Type | Access |
|------|------|--------|
| `bus` | `PciBus` | public |

### Methods

#### `function PciScanner( self, I64 capacity ) -> Void`

Construct a PCI scanner that can hold up to capacity discovered devices.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function scan( self ) -> Void`

Enumerate all devices on the PCI bus by probing every bus/device/function combination. Populates the internal device list.

#### `function getDeviceCount( self ) -> I64`

Return the number of PCI devices discovered during the last scan. 

#### `function findByVendorDevice( self, I64 vendorId, I64 deviceId ) -> I64`

Search for a PCI device by vendor and device ID. Returns the device index or -1 if not found.

#### `function findByClass( self, I64 classCode, I64 subclass ) -> I64`

Search for a PCI device by class code and subclass. Returns the device index or -1 if not found.

#### `function destroy( self ) -> Void`

Release all resources held by the scanner.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

