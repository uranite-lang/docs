# uranite.os.security.handle

## Table of Contents

- [Imports](#imports)
- [struct `HandleEntry`](#struct-handleentry)
  - [`HandleEntry()`](#HandleEntry)
- [class `HandleTable`](#class-handletable)
  - [`HandleTable()`](#HandleTable)
  - [`allocate()`](#allocate)
  - [`lookup()`](#lookup)
  - [`close()`](#close)
  - [`duplicate()`](#duplicate)
  - [`getCount()`](#getCount)
  - [`getCapacity()`](#getCapacity)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.memory.object`
  - `KernelObject`
- `uranite.os.security.capability`
  - `Capability`

## struct `HandleEntry`

Represents a single slot in a handle table, pairing a kernel object reference with its capability mask. A None object field indicates a free slot. The nextFree field chains free slots together into an intrusive free list for O(1) slot allocation.

### Fields

| Name | Type | Access |
|------|------|--------|
| `object` | `?KernelObject` | public |
| `capability` | `Capability` | public |
| `nextFree` | `I64` | public |

### Methods

#### `function HandleEntry( self ) -> Void`

Constructs a new empty handle entry with no associated kernel object, an empty capability mask (no permissions), and the free-list pointer set to -1 (end of list).

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## class `HandleTable`

Per-process mapping from integer handle IDs to kernel objects with associated capability masks. The table has a fixed capacity and uses free-list chaining for O(1) handle allocation and deallocation. Handle IDs are indices into the table array. When a handle is closed, the kernel object's reference count is decremented and the slot is returned to the free list. Handle duplication supports capability attenuation (reducing permissions) but prevents privilege escalation.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `Memory<HandleEntry>` | protect |
| `capacity` | `I64` | protect |
| `count` | `I64` | protect |
| `firstFree` | `I64` | protect |

### Methods

#### `function HandleTable( self, I64 capacity ) -> Void`

Constructs a new handle table with the specified capacity. All slots are initialized as empty HandleEntry objects and chained together into a free list, with each slot pointing to the next and the last slot marking the end of the list with -1.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function allocate( self, KernelObject object, Capability capability ) -> I64`

Allocates a new handle for the given kernel object with the specified capability mask. The kernel object's reference count is incremented to reflect the new handle reference. Returns the handle ID (slot index) on success, or -1 if the handle table is full.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function lookup( self, I64 handleId ) -> ?HandleEntry`

Looks up a handle by its ID and returns the associated HandleEntry containing the kernel object reference and capability mask. Returns None if the handle ID is out of bounds or the slot is not allocated.

#### `function close( self, I64 handleId ) -> Boolean`

Closes a handle, releasing the kernel object reference by decrementing its reference count. The handle slot is cleared and returned to the free list for reuse. Returns True if the kernel object's reference count reached zero, indicating the caller should destroy the object. Returns False if the handle ID is invalid, the slot is empty, or other references to the object remain.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function duplicate( self, I64 handleId, Capability newCapability ) -> I64`

Duplicates a handle with attenuated (reduced) capabilities. The new capability must be a subset of the original handle's capability to prevent privilege escalation. A new handle is allocated for the same kernel object with the attenuated capability. Returns the new handle ID on success, or -1 if the original handle is invalid, the slot is empty, or the new capability is not a subset of the original.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Returns the number of currently allocated handles in the table. 

#### `function getCapacity( self ) -> I64`

Returns the maximum number of handles this table can hold. 

#### `function destroy( self ) -> Void`

Releases the heap-allocated memory buffer used to store handle entries.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

