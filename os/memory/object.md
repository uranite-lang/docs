# uranite.os.memory.object

## Table of Contents

- [Imports](#imports)
- [enum `ObjectType`](#enum-objecttype)
- [interface `ObjectOps`](#interface-objectops)
  - [`read()`](#read)
  - [`write()`](#write)
  - [`close()`](#close)
  - [`ioctl()`](#ioctl)
- [class `KernelObject`](#class-kernelobject)
  - [`KernelObject()`](#KernelObject)
  - [`retain()`](#retain)
  - [`release()`](#release)
  - [`getId()`](#getId)
  - [`getType()`](#getType)
  - [`getRefCount()`](#getRefCount)
  - [`getOps()`](#getOps)

## Imports

- `uranite.memory.memory`
  - `Memory`

## enum `ObjectType`

Categorizes kernel-managed resources for type-safe dispatch. Each variant represents a distinct resource type managed by the kernel's object system.

## interface `ObjectOps`

Polymorphic operation table that each kernel resource type implements. Provides a uniform interface for reading, writing, closing, and performing I/O control operations on any kernel object regardless of its concrete type.

### Methods

#### `function read( self, Memory<I8> buffer, I64 offset, I64 size ) -> I64`

Read up to the specified number of bytes from the resource into the buffer at the given offset. Returns the number of bytes read. 

#### `function write( self, Memory<I8> buffer, I64 offset, I64 size ) -> I64`

Write the specified number of bytes from the buffer to the resource at the given offset. Returns the number of bytes written. 

#### `function close( self ) -> I32`

Close the resource and release any associated system resources. Returns 0 on success or an error code. 

#### `function ioctl( self, I64 command, I64 argument ) -> I64`

Perform a device-specific I/O control operation identified by the command code with the given argument. Returns an operation-specific result value.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `KernelObject`

Base representation for all kernel-managed resources. Every file, socket, process, device, and other resource in the kernel is wrapped in a KernelObject that tracks a unique identifier, resource type, reference count, and a polymorphic operation table. Reference counting controls the object's lifetime: when the count reaches zero, the object may be destroyed.

### Fields

| Name | Type | Access |
|------|------|--------|
| `id` | `I64` | protect |
| `objType` | `ObjectType` | protect |
| `refCount` | `I64` | protect |
| `ops` | `ObjectOps` | protect |

### Methods

#### `function KernelObject( self, I64 id, ObjectType objType, ObjectOps ops ) -> Void`

Construct a kernel object with the given unique identifier, resource type, and operation table. The initial reference count is set to 1, representing the creating handle's ownership.

#### `function retain( self ) -> Void`

Increment the reference count by one, indicating that a new handle or reference to this object has been created.

#### `function release( self ) -> Boolean`

Decrement the reference count by one and return True if the count has reached zero or below, indicating that the object has no remaining references and may be safely destroyed.

#### `function getId( self ) -> I64`

Return the unique identifier of this kernel object. 

#### `function getType( self ) -> ObjectType`

Return the resource type of this kernel object. 

#### `function getRefCount( self ) -> I64`

Return the current reference count of this kernel object. 

#### `function getOps( self ) -> ObjectOps`

Return the polymorphic operation table for this kernel object.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

