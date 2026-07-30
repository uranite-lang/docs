# uranite.os.vfs.mount

## Table of Contents

- [Imports](#imports)
- [class `MountEntry`](#class-mountentry)
  - [`MountEntry()`](#MountEntry)
  - [`isReadOnly()`](#isReadOnly)
  - [`getMountId()`](#getMountId)
  - [`getFilesystemId()`](#getFilesystemId)
  - [`getRootInode()`](#getRootInode)
- [const `MOUNT_READ_ONLY`](#const-mount-read-only)
- [const `MOUNT_NO_EXEC`](#const-mount-no-exec)
- [const `MOUNT_NO_SUID`](#const-mount-no-suid)
- [class `MountTable`](#class-mounttable)
  - [`MountTable()`](#MountTable)
  - [`mount()`](#mount)
  - [`unmount()`](#unmount)
  - [`findByMountPoint()`](#findByMountPoint)
  - [`getRootInode()`](#getRootInode)
  - [`isMountPoint()`](#isMountPoint)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `MountEntry`

Represents a single mount point that binds a filesystem to a directory in the VFS tree. Each mount maps a VNode ID (the mount point directory) to a filesystem instance identified by its filesystem ID and root inode number. Mount flags control access restrictions such as read-only or no-execute.

### Fields

| Name | Type | Access |
|------|------|--------|
| `mountId` | `I64` | public |
| `mountPointVnodeId` | `I64` | public |
| `filesystemId` | `I64` | public |
| `rootInode` | `I64` | public |
| `filesystemType` | `I64` | public |
| `flags` | `I64` | public |

### Methods

#### `function MountEntry( self, I64 id, I64 mountVnode, I64 fsId, I64 rootIn, I64 fsType, I64 mountFlags ) -> Void`

Constructs a new mount entry with the specified mount ID, mount point VNode ID, filesystem ID, root inode number, filesystem type, and mount flags.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isReadOnly( self ) -> Boolean`

Returns True if this mount was created with the read-only flag (bit 0). 

#### `function getMountId( self ) -> I64`

Returns the unique identifier assigned to this mount point. 

#### `function getFilesystemId( self ) -> I64`

Returns the filesystem instance identifier associated with this mount. 

#### `function getRootInode( self ) -> I64`

Returns the root inode number of the mounted filesystem.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## const `MOUNT_READ_ONLY`

Mount flag for read-only. No write operations permitted on mounted filesystem. 

## const `MOUNT_NO_EXEC`

Mount flag for no-execute. Execution of files on mounted filesystem prohibited. 

## const `MOUNT_NO_SUID`

Mount flag for no-setuid. Set-user-ID and set-group-ID bits are ignored. 

## class `MountTable`

Global table that tracks all active filesystem mounts in the system. Each entry maps a VNode ID (mount point directory) to a filesystem instance and its root inode. The table assigns monotonically increasing mount IDs and uses a spinlock for thread-safe concurrent access during mount and unmount operations.

### Fields

| Name | Type | Access |
|------|------|--------|
| `mountIds` | `Memory<I64>` | protect |
| `mountPoints` | `Memory<I64>` | protect |
| `fsIds` | `Memory<I64>` | protect |
| `rootInodes` | `Memory<I64>` | protect |
| `fsTypes` | `Memory<I64>` | protect |
| `mountFlags` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `count` | `I64` | protect |
| `nextMountId` | `I64` | protect |
| `lock` | `Spinlock` | protect |

### Methods

#### `function MountTable( self, I64 maxMounts ) -> Void`

Constructs a new mount table with the specified maximum number of mount entries. All slots are initialized to zero and the next mount ID starts at 1.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function mount( self, I64 mountPointVnode, I64 fsId, I64 rootInode, I64 fsType, I64 flags ) -> I64`

Mounts a filesystem at the specified VNode directory. A unique mount ID is assigned and the filesystem ID, root inode, type, and flags are recorded. Returns the mount ID on success, or -1 if the mount table is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unmount( self, I64 mountId ) -> Boolean`

Unmounts the filesystem with the given mount ID by clearing its slot in the table. Returns True if the mount was found and removed, or False if no mount with the given ID exists.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findByMountPoint( self, I64 vnodeId ) -> I64`

Searches for a filesystem mounted at the specified VNode ID. Returns the filesystem ID if a mount exists at this VNode, or -1 if the VNode is not a mount point.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getRootInode( self, I64 vnodeId ) -> I64`

Returns the root inode number of the filesystem mounted at the specified VNode ID. Returns -1 if no filesystem is mounted at this VNode.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function isMountPoint( self, I64 vnodeId ) -> Boolean`

Checks whether the specified VNode ID is currently used as a mount point for any filesystem. Returns True if a mount exists at this VNode, or False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Returns the number of currently active mounts in the mount table. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the mount table, including the mount ID, mount point, filesystem ID, root inode, filesystem type, and mount flags arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

