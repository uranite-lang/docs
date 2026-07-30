# uranite.os.security.capability

## Table of Contents

- [enum `CapabilityFlag`](#enum-capabilityflag)
- [class `Capability`](#class-capability)
  - [`Capability()`](#Capability)
  - [`has()`](#has)
  - [`add()`](#add)
  - [`remove()`](#remove)
  - [`contains()`](#contains)
  - [`merge()`](#merge)
  - [`intersect()`](#intersect)
  - [`getMask()`](#getMask)
  - [`isEmpty()`](#isEmpty)

## enum `CapabilityFlag`

Named permission flags for capability-based access control. Each flag maps to a single bit position in the capability bitmask and represents a distinct operation that can be granted or denied on a kernel object. Codegen assigns sequential values (0, 1, 2, ...) used as bit indices.

## class `Capability`

Bitmask-based capability token that controls access to kernel objects. Capabilities are unforgeable: only the kernel can create or modify them. Userspace receives opaque handle IDs that reference capabilities stored in the kernel's handle table. Each bit in the mask corresponds to a CapabilityFlag, enabling fine-grained permission control through set operations (union, intersection, difference).

### Fields

| Name | Type | Access |
|------|------|--------|
| `mask` | `I64` | protect |

### Methods

#### `function Capability( self, I64 mask ) -> Void`

Constructs a new capability with the specified permission bitmask. Each set bit in the mask grants the corresponding permission flag defined by CapabilityFlag.

#### `function has( self, I64 flagBit ) -> Boolean`

Checks whether this capability includes the specified permission flag. Returns True if the flag bit is set in the capability mask, indicating the permission is granted.

#### `function add( self, I64 flagBit ) -> Capability`

Returns a new capability with the specified permission flag added to the existing permission set. The original capability is not modified.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function remove( self, I64 flagBit ) -> Capability`

Returns a new capability with the specified permission flag removed from the existing permission set. The original capability is not modified.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function contains( self, Capability other ) -> Boolean`

Checks whether the other capability is a subset of this capability, meaning all permissions in the other capability are also present in this one. This check prevents privilege escalation when duplicating or attenuating capabilities.

#### `function merge( self, Capability other ) -> Capability`

Returns a new capability representing the union of this capability and the other capability. The resulting capability has all permissions from both operands.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function intersect( self, Capability other ) -> Capability`

Returns a new capability representing the intersection of this capability and the other capability. The resulting capability only has permissions present in both operands.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getMask( self ) -> I64`

Returns the raw permission bitmask of this capability. 

#### `function isEmpty( self ) -> Boolean`

Returns True if this capability grants no permissions (mask is zero).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

