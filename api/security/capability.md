# uranite.security.capability

## Table of Contents

- [Imports](#imports)
- [class `CapabilitySet`](#class-capabilityset)
  - [`CapabilitySet()`](#CapabilitySet)
  - [`grant()`](#grant)
  - [`revoke()`](#revoke)
  - [`check()`](#check)
  - [`containsAll()`](#containsAll)
  - [`mergeWith()`](#mergeWith)
  - [`intersectWith()`](#intersectWith)
  - [`getMask()`](#getMask)
  - [`isEmpty()`](#isEmpty)
  - [`getCapability()`](#getCapability)

## Imports

- `uranite.os.security.capability`
  - `Capability`
  - `CapabilityFlag`

## class `CapabilitySet`

Manages a mutable set of security capabilities for permission checking. Wraps the immutable kernel Capability with grant/revoke operations that replace the internal capability on each mutation. Supports subset checks for privilege escalation prevention.

### Fields

| Name | Type | Access |
|------|------|--------|
| `capabilities` | `Capability` | protect |

### Methods

#### `function CapabilitySet( self ) -> Void`

Construct an empty capability set with no permissions granted.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function grant( self, I64 flagBit ) -> Void`

Grant a permission flag to this capability set.

**Parameters**:

- `flagBit` (`I64`)
- `Bit value of the permission to grant.`

#### `function revoke( self, I64 flagBit ) -> Void`

Revoke a permission flag from this capability set.

**Parameters**:

- `flagBit` (`I64`)
- `Bit value of the permission to revoke.`

#### `function check( self, I64 flagBit ) -> Boolean`

Check whether a specific permission is granted.

**Parameters**:

- `flagBit` (`I64`)
- `Bit value of the permission to check.`

**Returns**: `Boolean` — True if the permission is present in this set.

#### `function containsAll( self, Capability other ) -> Boolean`

Check whether this set contains all permissions from another capability. Used to prevent privilege escalation during handle duplication.

**Parameters**:

- `other` (`Capability`)
- `Capability whose permissions must be a subset.`

**Returns**: `Boolean` — True if all permissions in other are present here.

#### `function mergeWith( self, Capability other ) -> Void`

Merge permissions from another capability into this set.

**Parameters**:

- `other` (`Capability`)
- `Capability whose permissions to add.`

#### `function intersectWith( self, Capability other ) -> Void`

Reduce this set to only permissions also present in other.

**Parameters**:

- `other` (`Capability`)
- `Capability to intersect with.`

#### `function getMask( self ) -> I64`

Return the raw permission bitmask. 

#### `function isEmpty( self ) -> Boolean`

Return whether no permissions are granted. 

#### `function getCapability( self ) -> Capability`

Return the underlying immutable Capability snapshot.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

