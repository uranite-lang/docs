# uranite.os.fs.uranitefs

## Table of Contents

- [Imports](#imports)
- [class `UraniteFsSuperblock`](#class-uranitefssuperblock)
  - [`UraniteFsSuperblock()`](#UraniteFsSuperblock)
  - [`isValid()`](#isValid)
- [class `UraniteFsInode`](#class-uranitefsinode)
  - [`UraniteFsInode()`](#UraniteFsInode)
  - [`isDirectory()`](#isDirectory)
  - [`isRegularFile()`](#isRegularFile)
  - [`hasCapability()`](#hasCapability)
- [class `UraniteFileSystem`](#class-uranitefilesystem)
  - [`UraniteFileSystem()`](#UraniteFileSystem)
  - [`read()`](#read)
  - [`write()`](#write)
  - [`lookup()`](#lookup)
  - [`create()`](#create)
  - [`remove()`](#remove)
  - [`getSize()`](#getSize)
  - [`getType()`](#getType)
  - [`getPermissions()`](#getPermissions)
  - [`sync()`](#sync)
  - [`isValid()`](#isValid)
  - [`getDeviceId()`](#getDeviceId)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.vfs.filesystem`
  - `FileSystem`

## class `UraniteFsSuperblock`

UraniteFS superblock containing filesystem-level metadata including the magic number for identification, format version, block size, total and free block/inode counts, and journal location. The magic number 0x41455448 ("AETH") is used to validate the superblock on mount.

### Fields

| Name | Type | Access |
|------|------|--------|
| `magic` | `I64` | public |
| `version` | `I64` | public |
| `blockSize` | `I64` | public |
| `totalBlocks` | `I64` | public |
| `totalInodes` | `I64` | public |
| `freeBlocks` | `I64` | public |
| `freeInodes` | `I64` | public |
| `journalStart` | `I64` | public |
| `journalSize` | `I64` | public |

### Methods

#### `function UraniteFsSuperblock( self ) -> Void`

Construct an UraniteFS superblock with default values: magic number 0x41455448, version 1, 4096-byte block size, and all counters at zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isValid( self ) -> Boolean`

Return whether this superblock has a valid UraniteFS magic number.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `UraniteFsInode`

UraniteFS inode that extends traditional POSIX metadata with a capability token for capability-based access control. Stores file mode, ownership (uid/gid), size, link count, timestamps, and a pointer to the first data block.

### Fields

| Name | Type | Access |
|------|------|--------|
| `mode` | `I64` | public |
| `uid` | `I64` | public |
| `gid` | `I64` | public |
| `size` | `I64` | public |
| `linkCount` | `I64` | public |
| `capabilityToken` | `I64` | public |
| `createdTime` | `I64` | public |
| `modifiedTime` | `I64` | public |
| `dataBlock` | `I64` | public |

### Methods

#### `function UraniteFsInode( self ) -> Void`

Construct an UraniteFS inode with default values. The link count defaults to 1 and all other fields are initialized to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isDirectory( self ) -> Boolean`

Return whether this inode represents a directory (mode bit 14 set, S_IFDIR = 0x4000). 

#### `function isRegularFile( self ) -> Boolean`

Return whether this inode represents a regular file (mode bit 15 set, S_IFREG = 0x8000). 

#### `function hasCapability( self ) -> Boolean`

Return whether this inode has a capability token assigned for access control.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `UraniteFileSystem`

**Implements**: `FileSystem`

UraniteFS filesystem driver skeleton implementing the FileSystem interface for integration with the Virtual File System layer. Provides stub implementations for read, write, lookup, create, remove, and sync operations that will be completed with actual disk I/O logic.

### Fields

| Name | Type | Access |
|------|------|--------|
| `superblock` | `UraniteFsSuperblock` | protect |
| `deviceId` | `I64` | protect |
| `filesystemId` | `I64` | protect |

### Methods

#### `function UraniteFileSystem( self, I64 devId, I64 fsId ) -> Void`

Construct an UraniteFS filesystem driver bound to the specified device and filesystem IDs. Initializes the superblock with default values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function read( self, I64 inodeNumber, Memory<I8> buffer, I64 offset, I64 length ) -> I64`

Read data from the file identified by inode number into the buffer starting at the given offset for the specified length. Returns the number of bytes read. Currently returns 0 as a stub implementation.

#### `function write( self, I64 inodeNumber, Memory<I8> buffer, I64 offset, I64 length ) -> I64`

Write data from the buffer to the file identified by inode number starting at the given offset for the specified length. Returns the number of bytes written. Currently returns 0 as a stub implementation.

#### `function lookup( self, I64 directoryInode, Memory<I8> name, I64 nameLength ) -> I64`

Look up a file or directory by name within the directory identified by its inode number. Returns the inode number of the found entry, or -1 if not found. Currently returns -1 as a stub implementation.

#### `function create( self, I64 directoryInode, Memory<I8> name, I64 nameLength, I64 nodeType ) -> I64`

Create a new file or directory entry with the given name and type within the directory identified by its inode number. Returns the inode number of the created entry, or -1 on failure. Currently returns -1 as a stub implementation.

#### `function remove( self, I64 directoryInode, Memory<I8> name, I64 nameLength ) -> Boolean`

Remove a file or directory entry by name from the directory identified by its inode number. Returns True on success, False on failure. Currently returns False as a stub implementation.

#### `function getSize( self, I64 inodeNumber ) -> I64`

Return the size in bytes of the file identified by inode number. Currently returns 0 as a stub implementation.

#### `function getType( self, I64 inodeNumber ) -> I64`

Return the type of the node identified by inode number (regular file, directory, etc.). Currently returns 1 as a stub implementation.

#### `function getPermissions( self, I64 inodeNumber ) -> I64`

Return the permission bits of the node identified by inode number. Currently returns 448 (octal 0700, owner read/write/execute) as a stub implementation.

#### `function sync( self ) -> Void`

Flush all pending writes and metadata changes to the underlying storage device. Currently a no-op stub implementation.

#### `function isValid( self ) -> Boolean`

Return whether the filesystem superblock has a valid UraniteFS magic number. 

#### `function getDeviceId( self ) -> I64`

Return the device ID this filesystem is mounted on.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

