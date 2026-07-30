# uranite.io.stat

## Table of Contents

- [Imports](#imports)
- [enum `FileType`](#enum-filetype)
- [class `FileStat`](#class-filestat)
  - [`FileStat()`](#FileStat)
  - [`getDeviceId()`](#getDeviceId)
  - [`getInodeNumber()`](#getInodeNumber)
  - [`getLinkCount()`](#getLinkCount)
  - [`getMode()`](#getMode)
  - [`getUid()`](#getUid)
  - [`getGid()`](#getGid)
  - [`getSize()`](#getSize)
  - [`getBlockSize()`](#getBlockSize)
  - [`getBlocks()`](#getBlocks)
  - [`getAccessTime()`](#getAccessTime)
  - [`getModifyTime()`](#getModifyTime)
  - [`getChangeTime()`](#getChangeTime)
  - [`getFileType()`](#getFileType)
  - [`isFile()`](#isFile)
  - [`isDirectory()`](#isDirectory)
  - [`isSymlink()`](#isSymlink)
- [function `parseStatBuffer`](#function-parsestatbuffer)
- [function `statPath`](#function-statpath)
  - [`statPath()`](#statPath)
- [function `lstatPath`](#function-lstatpath)
  - [`lstatPath()`](#lstatPath)
- [function `fstatFd`](#function-fstatfd)
  - [`fstatFd()`](#fstatFd)

## Imports

- `uranite.io.syscall`
  - `S_IFBLK`
  - `S_IFCHR`
  - `S_IFDIR`
  - `S_IFIFO`
  - `S_IFLNK`
  - `S_IFMT`
  - `S_IFREG`
  - `S_IFSOCK`
  - `memoryToPtr`
  - `readI32At`
  - `readI64At`
  - `stringToPtr`
  - `sysFstat`
  - `sysLstat`
  - `sysStat`
- `uranite.memory.memory`
  - `Memory`

## enum `FileType`

Enumeration of filesystem entry types as identified by the mode field of a stat structure. Values correspond to the S_IFMT mask results from the Linux stat syscall.

## class `FileStat`

Immutable representation of file status information obtained from the Linux stat, lstat, or fstat syscalls. Parses the x86_64 struct stat layout (144 bytes) into typed fields for device, inode, mode, ownership, size, and timestamps.

### Fields

| Name | Type | Access |
|------|------|--------|
| `deviceId` | `I64` | public |
| `inodeNumber` | `I64` | public |
| `linkCount` | `I64` | public |
| `mode` | `I64` | public |
| `uid` | `I64` | public |
| `gid` | `I64` | public |
| `size` | `I64` | public |
| `blockSize` | `I64` | public |
| `blocks` | `I64` | public |
| `accessTime` | `I64` | public |
| `modifyTime` | `I64` | public |
| `changeTime` | `I64` | public |

### Methods

#### `function FileStat( self, I64 deviceId, I64 inodeNumber, I64 linkCount, I64 mode, I64 uid, I64 gid, I64 size, I64 blockSize, I64 blocks, I64 accessTime, I64 modifyTime, I64 changeTime ) -> Void`

Construct a FileStat with all stat fields populated directly from parsed syscall output.

**Parameters**:

- `deviceId` (`I64`)
- `The device ID of the filesystem containing the file.`
- `inodeNumber` (`I64`)
- `The inode number of the file.`
- `linkCount` (`I64`)
- `The number of hard links to the file.`
- `mode` (`I64`)
- `The raw mode bits encoding type and permissions.`
- `uid` (`I64`)
- `The user ID of the file owner.`
- `gid` (`I64`)
- `The group ID of the file owner.`
- `size` (`I64`)
- `The file size in bytes.`
- `blockSize` (`I64`)
- `The preferred I/O block size.`
- `blocks` (`I64`)
- `The number of 512-byte blocks allocated.`
- `accessTime` (`I64`)
- `The last access time as Unix epoch seconds.`
- `modifyTime` (`I64`)
- `The last modification time as Unix epoch seconds.`
- `changeTime` (`I64`)
- `The last status change time as Unix epoch seconds.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getDeviceId( self ) -> I64`

Return the device ID of the filesystem containing the file.

**Returns**: `I64` — The device identifier.

#### `function getInodeNumber( self ) -> I64`

Return the inode number uniquely identifying the file within its filesystem.

**Returns**: `I64` — The inode number.

#### `function getLinkCount( self ) -> I64`

Return the number of hard links pointing to this file.

**Returns**: `I64` — The hard link count.

#### `function getMode( self ) -> I64`

Return the raw file mode bits encoding both the file type and permission bits.

**Returns**: `I64` — The raw mode value from the stat structure.

#### `function getUid( self ) -> I64`

Return the user ID of the file owner.

**Returns**: `I64` — The owner's user ID.

#### `function getGid( self ) -> I64`

Return the group ID of the file owner.

**Returns**: `I64` — The owner's group ID.

#### `function getSize( self ) -> I64`

Return the file size in bytes.

**Returns**: `I64` — The file size.

#### `function getBlockSize( self ) -> I64`

Return the preferred block size for filesystem I/O operations on this file.

**Returns**: `I64` — The optimal I/O block size in bytes.

#### `function getBlocks( self ) -> I64`

Return the number of 512-byte blocks allocated for the file on disk.

**Returns**: `I64` — The allocated block count.

#### `function getAccessTime( self ) -> I64`

Return the last access time of the file.

**Returns**: `I64` — The access time as a Unix epoch timestamp in seconds.

#### `function getModifyTime( self ) -> I64`

Return the last modification time of the file content.

**Returns**: `I64` — The modification time as a Unix epoch timestamp in seconds.

#### `function getChangeTime( self ) -> I64`

Return the last status change time of the file metadata.

**Returns**: `I64` — The status change time as a Unix epoch timestamp in seconds.

#### `function getFileType( self ) -> FileType`

Determine the file type by masking the mode bits with S_IFMT and mapping the result to a FileType enum variant.

**Returns**: — FileType:
The type of the filesystem entry, or FileType.Unknown
if the type is not recognized.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isFile( self ) -> Boolean`

Check whether this entry is a regular file.

**Returns**: `Boolean` — True if the file type is S_IFREG.

#### `function isDirectory( self ) -> Boolean`

Check whether this entry is a directory.

**Returns**: `Boolean` — True if the file type is S_IFDIR.

#### `function isSymlink( self ) -> Boolean`

Check whether this entry is a symbolic link. Only meaningful when the FileStat was obtained via lstatPath, since statPath follows symlinks.

**Returns**: — Boolean:
True if the file type is S_IFLNK.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `parseStatBuffer`

Parse a raw x86_64 struct stat buffer into a FileStat object by reading fields at their fixed byte offsets. The buffer must be at least 144 bytes (18 I64 words).

**Parameters**:

- `bufAddr` (`I64`)
- `The memory address of the raw stat buffer.`

**Returns**: `FileStat` — A new FileStat populated with all parsed fields.

### Methods

#### `function parseStatBuffer( I64 bufAddr ) -> FileStat`

Parse a raw x86_64 struct stat buffer into a FileStat object by reading fields at their fixed byte offsets. The buffer must be at least 144 bytes (18 I64 words).

**Parameters**:

- `bufAddr` (`I64`)
- `The memory address of the raw stat buffer.`

**Returns**: `FileStat` — A new FileStat populated with all parsed fields.

## function `statPath`

Obtain file status information for the given path using the stat syscall. Follows symbolic links to report on the target file.

**Parameters**:

- `path` (`String`)
- `The filesystem path to stat.`

**Returns**: `FileStat` — The file status information.

**Raises**:

- `IOError` → `Error` — If the stat syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function statPath( String path ) -> FileStat`

Obtain file status information for the given path using the stat syscall. Follows symbolic links to report on the target file.

**Parameters**:

- `path` (`String`)
- `The filesystem path to stat.`

**Returns**: `FileStat` — The file status information.

**Raises**:

- `IOError` → `Error` — If the stat syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `lstatPath`

Obtain file status information for the given path using the lstat syscall. Does not follow symbolic links, so the returned FileStat describes the link itself rather than its target.

**Parameters**:

- `path` (`String`)
- `The filesystem path to lstat.`

**Returns**: `FileStat` — The file status information for the link or file.

**Raises**:

- `IOError` → `Error` — If the lstat syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function lstatPath( String path ) -> FileStat`

Obtain file status information for the given path using the lstat syscall. Does not follow symbolic links, so the returned FileStat describes the link itself rather than its target.

**Parameters**:

- `path` (`String`)
- `The filesystem path to lstat.`

**Returns**: `FileStat` — The file status information for the link or file.

**Raises**:

- `IOError` → `Error` — If the lstat syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `fstatFd`

Obtain file status information for an open file descriptor using the fstat syscall.

**Parameters**:

- `fd` (`I64`)
- `The open file descriptor to stat.`

**Returns**: `FileStat` — The file status information.

**Raises**:

- `IOError` → `Error` — If the fstat syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fstatFd( I64 fd ) -> FileStat`

Obtain file status information for an open file descriptor using the fstat syscall.

**Parameters**:

- `fd` (`I64`)
- `The open file descriptor to stat.`

**Returns**: `FileStat` — The file status information.

**Raises**:

- `IOError` → `Error` — If the fstat syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

