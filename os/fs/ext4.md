# uranite.os.fs.ext4

## Table of Contents

- [Imports](#imports)
- [class `Ext4Superblock`](#class-ext4superblock)
  - [`Ext4Superblock()`](#Ext4Superblock)
  - [`getBlockGroupForInode()`](#getBlockGroupForInode)
  - [`getInodeIndexInGroup()`](#getInodeIndexInGroup)
- [class `Ext4Inode`](#class-ext4inode)
  - [`Ext4Inode()`](#Ext4Inode)
  - [`isDirectory()`](#isDirectory)
  - [`isRegularFile()`](#isRegularFile)
  - [`isSymlink()`](#isSymlink)
- [class `Ext4FileSystem`](#class-ext4filesystem)
  - [`Ext4FileSystem()`](#Ext4FileSystem)
  - [`read()`](#read)
  - [`write()`](#write)
  - [`lookup()`](#lookup)
  - [`create()`](#create)
  - [`remove()`](#remove)
  - [`getSize()`](#getSize)
  - [`getType()`](#getType)
  - [`getPermissions()`](#getPermissions)
  - [`sync()`](#sync)
  - [`getDeviceId()`](#getDeviceId)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.vfs.filesystem`
  - `FileSystem`

## class `Ext4Superblock`

In-memory representation of key fields from the ext4 on-disk superblock. Stores filesystem geometry including total inodes and blocks, block size, blocks and inodes per block group, first data block offset, inode size, and the total number of block groups. Provides methods for computing block group and inode index from an inode number.

### Fields

| Name | Type | Access |
|------|------|--------|
| `totalInodes` | `I64` | public |
| `totalBlocks` | `I64` | public |
| `blockSize` | `I64` | public |
| `blocksPerGroup` | `I64` | public |
| `inodesPerGroup` | `I64` | public |
| `firstDataBlock` | `I64` | public |
| `inodeSize` | `I64` | public |
| `groupCount` | `I64` | public |

### Methods

#### `function Ext4Superblock( self ) -> Void`

Construct an ext4 superblock with default geometry values: 4096-byte blocks, 32768 blocks per group, 8192 inodes per group, 256-byte inodes, and all counters at zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getBlockGroupForInode( self, I64 inode ) -> I64`

Compute the block group number that contains the given inode number. Inode numbers in ext4 are 1-based, so the computation subtracts 1 before dividing by the inodes-per-group count.

#### `function getInodeIndexInGroup( self, I64 inode ) -> I64`

Compute the zero-based index of the given inode within its block group. Used to locate the inode's position in the group's inode table.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Ext4Inode`

In-memory representation of ext4 inode metadata. Stores the file mode (type and permissions), ownership (uid/gid), size, block count, link count, and inode flags for a single filesystem entry.

### Fields

| Name | Type | Access |
|------|------|--------|
| `mode` | `I64` | public |
| `uid` | `I64` | public |
| `gid` | `I64` | public |
| `size` | `I64` | public |
| `blocks` | `I64` | public |
| `linkCount` | `I64` | public |
| `flags` | `I64` | public |

### Methods

#### `function Ext4Inode( self ) -> Void`

Construct an ext4 inode with all fields initialized to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isDirectory( self ) -> Boolean`

Return whether this inode represents a directory (S_IFDIR = 0x4000, bit 14 set). 

#### `function isRegularFile( self ) -> Boolean`

Return whether this inode represents a regular file (S_IFREG = 0x8000, bit 15 set). 

#### `function isSymlink( self ) -> Boolean`

Return whether this inode represents a symbolic link (S_IFLNK = 0xA000).

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Ext4FileSystem`

**Implements**: `FileSystem`

ext4 filesystem driver skeleton implementing the FileSystem interface for integration with the Virtual File System layer. Provides stub implementations for read, write, lookup, create, remove, and sync operations that will be completed with actual ext4 disk I/O logic.

### Fields

| Name | Type | Access |
|------|------|--------|
| `superblock` | `Ext4Superblock` | protect |
| `deviceId` | `I64` | protect |
| `filesystemId` | `I64` | protect |

### Methods

#### `function Ext4FileSystem( self, I64 devId, I64 fsId ) -> Void`

Construct an ext4 filesystem driver bound to the specified device and filesystem IDs. Initializes the superblock with default values.

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

Return the permission bits of the node identified by inode number. Currently returns 420 (octal 0644, owner read/write, group/other read) as a stub implementation.

#### `function sync( self ) -> Void`

Flush all pending writes and metadata changes to the underlying storage device. Currently a no-op stub implementation.

#### `function getDeviceId( self ) -> I64`

Return the device ID this filesystem is mounted on.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

