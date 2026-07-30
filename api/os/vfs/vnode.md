# uranite.os.vfs.vnode

## Table of Contents

- [Imports](#imports)
- [enum `VNodeType`](#enum-vnodetype)
- [const `PERM_READ`](#const-perm-read)
- [const `PERM_WRITE`](#const-perm-write)
- [const `PERM_EXECUTE`](#const-perm-execute)
- [const `PERM_OWNER_READ`](#const-perm-owner-read)
- [const `PERM_OWNER_WRITE`](#const-perm-owner-write)
- [const `PERM_OWNER_EXECUTE`](#const-perm-owner-execute)
- [class `VNode`](#class-vnode)
  - [`VNode()`](#VNode)
  - [`retain()`](#retain)
  - [`release()`](#release)
  - [`isFile()`](#isFile)
  - [`isDirectory()`](#isDirectory)
  - [`isSymlink()`](#isSymlink)
  - [`canOwnerRead()`](#canOwnerRead)
  - [`canOwnerWrite()`](#canOwnerWrite)
  - [`canOwnerExecute()`](#canOwnerExecute)
  - [`setPermissions()`](#setPermissions)
  - [`setSize()`](#setSize)
  - [`setOwner()`](#setOwner)
  - [`getVnodeId()`](#getVnodeId)
  - [`getFilesystemId()`](#getFilesystemId)
  - [`getInodeNumber()`](#getInodeNumber)
- [class `VNodeCache`](#class-vnodecache)
  - [`VNodeCache()`](#VNodeCache)
  - [`insert()`](#insert)
  - [`lookup()`](#lookup)
  - [`remove()`](#remove)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## enum `VNodeType`

Enumerates the types of nodes in the virtual filesystem. Each type corresponds to a distinct kind of filesystem object, from regular files and directories to special device nodes, pipes, and sockets.

## const `PERM_READ`

POSIX read permission for "other" category (bits 0-2). 

## const `PERM_WRITE`

POSIX write permission for "other" category (bits 0-2). 

## const `PERM_EXECUTE`

POSIX execute permission for "other" category (bits 0-2). 

## const `PERM_OWNER_READ`

POSIX owner read permission (bit 8). 

## const `PERM_OWNER_WRITE`

POSIX owner write permission (bit 7). 

## const `PERM_OWNER_EXECUTE`

POSIX owner execute permission (bit 6). 

## class `VNode`

In-memory representation of a file, directory, or special node in the virtual filesystem. VNodes are filesystem-agnostic, providing a unified abstraction over different underlying filesystem formats. Metadata (size, permissions, ownership, timestamps, link count) is cached from the underlying filesystem. Each VNode is reference-counted to track how many open file descriptors and directory entries reference it. The filesystem ID and inode number link the VNode back to its concrete storage in the underlying filesystem.

### Fields

| Name | Type | Access |
|------|------|--------|
| `vnodeId` | `I64` | public |
| `vnodeType` | `I64` | public |
| `size` | `I64` | public |
| `permissions` | `I64` | public |
| `ownerUid` | `I64` | public |
| `ownerGid` | `I64` | public |
| `refCount` | `I64` | public |
| `filesystemId` | `I64` | public |
| `inodeNumber` | `I64` | public |
| `createdTime` | `I64` | public |
| `modifiedTime` | `I64` | public |
| `accessedTime` | `I64` | public |
| `linkCount` | `I64` | public |

### Methods

#### `function VNode( self, I64 id, I64 nodeType, I64 fsId, I64 inode ) -> Void`

Constructs a new VNode with the given unique identifier, node type (from VNodeType), filesystem identifier, and underlying inode number. The VNode starts with a reference count of 1, link count of 1, and all other metadata fields (size, permissions, ownership, timestamps) initialized to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function retain( self ) -> I64`

Increments the VNode reference count by one and returns the new count. Called when a new file descriptor or directory entry references this VNode.

#### `function release( self ) -> Boolean`

Decrements the VNode reference count by one. Returns True if the reference count reached zero, indicating the VNode is no longer referenced and can be evicted from the cache and its resources freed.

#### `function isFile( self ) -> Boolean`

Returns True if this VNode represents a regular file (type 1). 

#### `function isDirectory( self ) -> Boolean`

Returns True if this VNode represents a directory (type 2). 

#### `function isSymlink( self ) -> Boolean`

Returns True if this VNode represents a symbolic link (type 3). 

#### `function canOwnerRead( self ) -> Boolean`

Checks whether the owner has read permission on this VNode by testing bit 8 (value 256) of the POSIX permission bitmask.

#### `function canOwnerWrite( self ) -> Boolean`

Checks whether the owner has write permission on this VNode by testing bit 7 (value 128) of the POSIX permission bitmask.

#### `function canOwnerExecute( self ) -> Boolean`

Checks whether the owner has execute permission on this VNode by testing bit 6 (value 64) of the POSIX permission bitmask.

#### `function setPermissions( self, I64 perms ) -> Void`

Sets the 12-bit POSIX permission bitmask for this VNode. 

#### `function setSize( self, I64 newSize ) -> Void`

Updates the cached file size for this VNode. 

#### `function setOwner( self, I64 uid, I64 gid ) -> Void`

Sets the owner user ID and group ID for this VNode. 

#### `function getVnodeId( self ) -> I64`

Returns the unique identifier assigned to this VNode. 

#### `function getFilesystemId( self ) -> I64`

Returns the filesystem identifier that this VNode belongs to. 

#### `function getInodeNumber( self ) -> I64`

Returns the underlying inode number of this VNode in its filesystem.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `VNodeCache`

Cache that maps VNode IDs to VNode pointers (stored as type-erased I64 values) for fast lookup. This avoids repeated filesystem queries for frequently accessed VNodes. The cache has a fixed capacity and uses linear scanning for lookups.

### Fields

| Name | Type | Access |
|------|------|--------|
| `ids` | `Memory<I64>` | protect |
| `pointers` | `Memory<I64>` | protect |
| `capacity` | `I64` | protect |
| `count` | `I64` | protect |

### Methods

#### `function VNodeCache( self, I64 maxEntries ) -> Void`

Constructs a new VNode cache with the specified maximum number of entries. All slots are initialized to empty (ID and pointer set to zero).

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function insert( self, I64 vnodeId, I64 vnodePointer ) -> Boolean`

Inserts a VNode into the cache by storing its ID and type-erased pointer in the first available slot. Returns True if the VNode was successfully cached, or False if the cache is full.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function lookup( self, I64 vnodeId ) -> I64`

Looks up a VNode in the cache by its ID. Returns the type-erased VNode pointer if found, or -1 if the VNode is not cached.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function remove( self, I64 vnodeId ) -> Boolean`

Removes a VNode from the cache by its ID, freeing the slot for reuse. Returns True if the VNode was found and removed, or False if it was not in the cache.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function getCount( self ) -> I64`

Returns the number of VNodes currently stored in the cache. 

#### `function destroy( self ) -> Void`

Releases all heap-allocated memory buffers used by the VNode cache, including the ID and pointer arrays.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

