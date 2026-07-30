# uranite.os.fs.fat32

## Table of Contents

- [Imports](#imports)
- [class `Fat32Info`](#class-fat32info)
  - [`Fat32Info()`](#Fat32Info)
  - [`clusterToSector()`](#clusterToSector)
  - [`getBytesPerCluster()`](#getBytesPerCluster)
- [class `Fat32DirEntry`](#class-fat32direntry)
  - [`Fat32DirEntry()`](#Fat32DirEntry)
  - [`isDirectory()`](#isDirectory)
  - [`isReadOnly()`](#isReadOnly)
  - [`isHidden()`](#isHidden)
  - [`isSystem()`](#isSystem)
  - [`destroy()`](#destroy)
- [class `Fat32FileSystem`](#class-fat32filesystem)
  - [`Fat32FileSystem()`](#Fat32FileSystem)
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
  - [`getRootCluster()`](#getRootCluster)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.vfs.filesystem`
  - `FileSystem`

## class `Fat32Info`

In-memory representation of FAT32 BIOS Parameter Block (BPB) fields needed to navigate the FAT32 filesystem. Stores sector and cluster geometry, reserved sector count, FAT table count and size, root directory cluster, and the computed data region start sector.

### Fields

| Name | Type | Access |
|------|------|--------|
| `bytesPerSector` | `I64` | public |
| `sectorsPerCluster` | `I64` | public |
| `reservedSectors` | `I64` | public |
| `numberOfFats` | `I64` | public |
| `sectorsPerFat` | `I64` | public |
| `rootCluster` | `I64` | public |
| `totalSectors` | `I64` | public |
| `dataStartSector` | `I64` | public |

### Methods

#### `function Fat32Info( self ) -> Void`

Construct a FAT32 info structure with standard default values: 512 bytes per sector, 8 sectors per cluster, 32 reserved sectors, 2 FAT copies, and root directory starting at cluster 2.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function clusterToSector( self, I64 cluster ) -> I64`

Convert a FAT32 cluster number to the corresponding sector number on disk. Cluster numbering starts at 2 (clusters 0 and 1 are reserved), so the computation subtracts 2 before multiplying by sectors per cluster.

#### `function getBytesPerCluster( self ) -> I64`

Return the number of bytes per cluster (bytes per sector times sectors per cluster).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `Fat32DirEntry`

In-memory representation of a FAT32 directory entry. The on-disk format is 32 bytes; this class expands it into accessible fields including the 8.3 filename stored as integer values, file attributes, starting cluster number, and file size.

### Fields

| Name | Type | Access |
|------|------|--------|
| `name` | `Memory<I64>` | public |
| `nameLength` | `I64` | public |
| `attributes` | `I64` | public |
| `firstCluster` | `I64` | public |
| `fileSize` | `I64` | public |

### Methods

#### `function Fat32DirEntry( self ) -> Void`

Construct a FAT32 directory entry with an 11-character name buffer (8.3 format) and all fields initialized to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isDirectory( self ) -> Boolean`

Return whether this entry is a directory (attribute bit 4 set). 

#### `function isReadOnly( self ) -> Boolean`

Return whether this entry is marked read-only (attribute bit 0 set). 

#### `function isHidden( self ) -> Boolean`

Return whether this entry is hidden (attribute bit 1 set). 

#### `function isSystem( self ) -> Boolean`

Return whether this entry is a system file (attribute bit 2 set). 

#### `function destroy( self ) -> Void`

Free the name buffer memory.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Fat32FileSystem`

**Implements**: `FileSystem`

FAT32 filesystem driver skeleton implementing the FileSystem interface for integration with the Virtual File System layer. Provides stub implementations for read, write, lookup, create, remove, and sync operations that will be completed with actual FAT32 disk I/O logic.

### Fields

| Name | Type | Access |
|------|------|--------|
| `info` | `Fat32Info` | protect |
| `deviceId` | `I64` | protect |
| `filesystemId` | `I64` | protect |

### Methods

#### `function Fat32FileSystem( self, I64 devId, I64 fsId ) -> Void`

Construct a FAT32 filesystem driver bound to the specified device and filesystem IDs. Initializes the BPB info with default values.

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

#### `function getDeviceId( self ) -> I64`

Return the device ID this filesystem is mounted on. 

#### `function getRootCluster( self ) -> I64`

Return the cluster number of the root directory.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

