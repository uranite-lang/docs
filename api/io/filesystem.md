# uranite.io.filesystem

## Table of Contents

- [Imports](#imports)
- [const `SYS_GETPID`](#const-sys-getpid)
- [function `copy`](#function-copy)
  - [`copy()`](#copy)
- [function `moveFile`](#function-movefile)
  - [`moveFile()`](#moveFile)
- [function `rename`](#function-rename)
  - [`rename()`](#rename)
- [function `remove`](#function-remove)
  - [`remove()`](#remove)
- [function `symlink`](#function-symlink)
  - [`symlink()`](#symlink)
- [function `hardlink`](#function-hardlink)
  - [`hardlink()`](#hardlink)
- [function `readlink`](#function-readlink)
  - [`readlink()`](#readlink)
- [function `lockShared`](#function-lockshared)
  - [`lockShared()`](#lockShared)
- [function `lockExclusive`](#function-lockexclusive)
  - [`lockExclusive()`](#lockExclusive)
- [function `unlock`](#function-unlock)
  - [`unlock()`](#unlock)
- [function `tryLockExclusive`](#function-trylockexclusive)
  - [`tryLockExclusive()`](#tryLockExclusive)
- [class `Pipe`](#class-pipe)
  - [`Pipe()`](#Pipe)
  - [`getReadFd()`](#getReadFd)
  - [`getWriteFd()`](#getWriteFd)
  - [`readBytes()`](#readBytes)
  - [`writeBytes()`](#writeBytes)
  - [`writeString()`](#writeString)
  - [`closeRead()`](#closeRead)
  - [`closeWrite()`](#closeWrite)
  - [`close()`](#close)
  - [`destroy()`](#destroy)
- [function `getTerminalSize`](#function-getterminalsize)
  - [`getTerminalSize()`](#getTerminalSize)
- [function `getTerminalRows`](#function-getterminalrows)
  - [`getTerminalRows()`](#getTerminalRows)
- [function `getTerminalCols`](#function-getterminalcols)
  - [`getTerminalCols()`](#getTerminalCols)
- [function `setRawMode`](#function-setrawmode)
  - [`setRawMode()`](#setRawMode)
- [function `restoreMode`](#function-restoremode)
  - [`restoreMode()`](#restoreMode)
- [function `chmod`](#function-chmod)
  - [`chmod()`](#chmod)
- [function `fchmod`](#function-fchmod)
  - [`fchmod()`](#fchmod)
- [function `chown`](#function-chown)
  - [`chown()`](#chown)
- [function `fchown`](#function-fchown)
  - [`fchown()`](#fchown)
- [class `FilesystemInfo`](#class-filesysteminfo)
  - [`FilesystemInfo()`](#FilesystemInfo)
  - [`getFsType()`](#getFsType)
  - [`getBlockSize()`](#getBlockSize)
  - [`getTotalBlocks()`](#getTotalBlocks)
  - [`getFreeBlocks()`](#getFreeBlocks)
  - [`getAvailableBlocks()`](#getAvailableBlocks)
  - [`getTotalFiles()`](#getTotalFiles)
  - [`getFreeFiles()`](#getFreeFiles)
  - [`getTotalSpace()`](#getTotalSpace)
  - [`getFreeSpace()`](#getFreeSpace)
  - [`getAvailableSpace()`](#getAvailableSpace)
- [function `statfs`](#function-statfs)
  - [`statfs()`](#statfs)
- [function `getcwd`](#function-getcwd)
  - [`getcwd()`](#getcwd)
- [function `chdir`](#function-chdir)
  - [`chdir()`](#chdir)
- [function `tempFile`](#function-tempfile)
  - [`tempFile()`](#tempFile)
- [function `tempDir`](#function-tempdir)
  - [`tempDir()`](#tempDir)
- [function `listDirectory`](#function-listdirectory)
  - [`listDirectory()`](#listDirectory)
- [function `makeDirectories`](#function-makedirectories)
  - [`makeDirectories()`](#makeDirectories)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.io.errors`
  - `IOError`
- `uranite.io.file`
  - `File`
- `uranite.io.path`
  - `join`
- `uranite.io.syscall`
  - `ECHO_FLAG`
  - `ICANON_FLAG`
  - `LOCK_EX`
  - `LOCK_NB`
  - `LOCK_SH`
  - `LOCK_UN`
  - `O_CLOEXEC`
  - `O_CREAT`
  - `O_EXCL`
  - `O_RDONLY`
  - `O_RDWR`
  - `O_TRUNC`
  - `O_WRONLY`
  - `SYS_FLOCK`
  - `SYS_GETDENTS64`
  - `SYS_MKDIR`
  - `SYS_OPEN`
  - `SYS_RENAME`
  - `TCGETS`
  - `TCSETS`
  - `TIOCGWINSZ`
  - `memoryToPtr`
  - `readByteAt`
  - `readI32At`
  - `readI64At`
  - `stringLen`
  - `stringToPtr`
  - `sysChdir`
  - `sysChmod`
  - `sysChown`
  - `sysClose`
  - `sysFchmod`
  - `sysFchown`
  - `sysFlock`
  - `sysGetcwd`
  - `sysGetdents64`
  - `sysIoctl`
  - `sysLink`
  - `sysMkdir`
  - `sysOpen`
  - `sysPipe2`
  - `sysRead`
  - `sysReadlink`
  - `sysRename`
  - `sysStatfs`
  - `sysSymlink`
  - `sysUnlink`
  - `sysWrite`
  - `writeByteAt`
  - `writeI32At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall0`
  - `syscall2`
  - `syscall3`
- `uranite.os.syscall.result`
  - `SyscallResult`
- `uranite.os.vfs.file-descriptor`
  - `FLAG_APPEND`
  - `FLAG_CREATE`
  - `FLAG_READ`
  - `FLAG_TRUNCATE`
  - `FLAG_WRITE`
  - `FileDescriptor`
  - `FileDescriptorTable`
- `uranite.os.vfs.mount`
  - `MOUNT_NO_EXEC`
  - `MOUNT_NO_SUID`
  - `MOUNT_READ_ONLY`
  - `MountEntry`
  - `MountTable`
- `uranite.os.vfs.vnode`
  - `PERM_EXECUTE`
  - `PERM_OWNER_EXECUTE`
  - `PERM_OWNER_READ`
  - `PERM_OWNER_WRITE`
  - `PERM_READ`
  - `PERM_WRITE`
  - `VNode`
  - `VNodeCache`
  - `VNodeType`

## const `SYS_GETPID`

## function `copy`

Copy a file from the source path to the destination path. Reads in 4096-byte chunks and writes them to the destination, handling partial writes. The destination file is created with permission mode 0644, or truncated if it already exists.

**Parameters**:

- `source` (`String`)
- `The filesystem path of the source file to copy.`
- `dest` (`String`)
- `The filesystem path of the destination file.`

**Raises**:

- `IOError` → `Error` — If the source cannot be read or the destination cannot be written.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function copy( String source, String dest ) -> Void`

Copy a file from the source path to the destination path. Reads in 4096-byte chunks and writes them to the destination, handling partial writes. The destination file is created with permission mode 0644, or truncated if it already exists.

**Parameters**:

- `source` (`String`)
- `The filesystem path of the source file to copy.`
- `dest` (`String`)
- `The filesystem path of the destination file.`

**Raises**:

- `IOError` → `Error` — If the source cannot be read or the destination cannot be written.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `moveFile`

Move a file from the source path to the destination path. Attempts an atomic rename first; if the rename fails (e.g., cross-device move), falls back to copy-then-delete.

**Parameters**:

- `source` (`String`)
- `The filesystem path of the file to move.`
- `dest` (`String`)
- `The target filesystem path.`

**Raises**:

- `IOError` → `Error` — If both the rename and the copy-delete fallback fail.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function moveFile( String source, String dest ) -> Void`

Move a file from the source path to the destination path. Attempts an atomic rename first; if the rename fails (e.g., cross-device move), falls back to copy-then-delete.

**Parameters**:

- `source` (`String`)
- `The filesystem path of the file to move.`
- `dest` (`String`)
- `The target filesystem path.`

**Raises**:

- `IOError` → `Error` — If both the rename and the copy-delete fallback fail.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `rename`

Rename a file or directory from the old path to the new path.

**Parameters**:

- `oldPath` (`String`)
- `The current filesystem path.`
- `newPath` (`String`)
- `The new filesystem path.`

**Raises**:

- `IOError` → `Error` — If the rename fails.

### Methods

#### `function rename( String oldPath, String newPath ) -> Void`

Rename a file or directory from the old path to the new path.

**Parameters**:

- `oldPath` (`String`)
- `The current filesystem path.`
- `newPath` (`String`)
- `The new filesystem path.`

**Raises**:

- `IOError` → `Error` — If the rename fails.

## function `remove`

Remove (unlink) a file at the specified path.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to remove.`

**Raises**:

- `IOError` → `Error` — If the file cannot be removed.

### Methods

#### `function remove( String path ) -> Void`

Remove (unlink) a file at the specified path.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to remove.`

**Raises**:

- `IOError` → `Error` — If the file cannot be removed.

## function `symlink`

Create a symbolic link at linkPath pointing to target.

**Parameters**:

- `target` (`String`)
- `The path that the symbolic link will point to.`
- `linkPath` (`String`)
- `The filesystem path where the symbolic link will be created.`

**Raises**:

- `IOError` → `Error` — If the symlink cannot be created.

### Methods

#### `function symlink( String target, String linkPath ) -> Void`

Create a symbolic link at linkPath pointing to target.

**Parameters**:

- `target` (`String`)
- `The path that the symbolic link will point to.`
- `linkPath` (`String`)
- `The filesystem path where the symbolic link will be created.`

**Raises**:

- `IOError` → `Error` — If the symlink cannot be created.

## function `hardlink`

Create a hard link at linkPath pointing to the same inode as source.

**Parameters**:

- `source` (`String`)
- `The existing file to create a hard link to.`
- `linkPath` (`String`)
- `The filesystem path where the hard link will be created.`

**Raises**:

- `IOError` → `Error` — If the hard link cannot be created.

### Methods

#### `function hardlink( String source, String linkPath ) -> Void`

Create a hard link at linkPath pointing to the same inode as source.

**Parameters**:

- `source` (`String`)
- `The existing file to create a hard link to.`
- `linkPath` (`String`)
- `The filesystem path where the hard link will be created.`

**Raises**:

- `IOError` → `Error` — If the hard link cannot be created.

## function `readlink`

Read the target path of a symbolic link.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the symbolic link to read.`

**Returns**: — The target path that the symbolic link points to.

**Raises**:

- `IOError` → `Error` — If the readlink fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readlink( String path ) -> String`

Read the target path of a symbolic link.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the symbolic link to read.`

**Returns**: — The target path that the symbolic link points to.

**Raises**:

- `IOError` → `Error` — If the readlink fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `lockShared`

Acquire a shared (read) lock on the given file descriptor. Multiple processes may hold a shared lock simultaneously. Blocks until the lock is acquired.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to lock.`

**Raises**:

- `IOError` → `Error` — If the lock operation fails.

### Methods

#### `function lockShared( I64 fd ) -> Void`

Acquire a shared (read) lock on the given file descriptor. Multiple processes may hold a shared lock simultaneously. Blocks until the lock is acquired.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to lock.`

**Raises**:

- `IOError` → `Error` — If the lock operation fails.

## function `lockExclusive`

Acquire an exclusive (write) lock on the given file descriptor. Only one process may hold an exclusive lock at a time. Blocks until the lock is acquired.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to lock.`

**Raises**:

- `IOError` → `Error` — If the lock operation fails.

### Methods

#### `function lockExclusive( I64 fd ) -> Void`

Acquire an exclusive (write) lock on the given file descriptor. Only one process may hold an exclusive lock at a time. Blocks until the lock is acquired.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to lock.`

**Raises**:

- `IOError` → `Error` — If the lock operation fails.

## function `unlock`

Release any lock (shared or exclusive) held on the given file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to unlock.`

**Raises**:

- `IOError` → `Error` — If the unlock operation fails.

### Methods

#### `function unlock( I64 fd ) -> Void`

Release any lock (shared or exclusive) held on the given file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to unlock.`

**Raises**:

- `IOError` → `Error` — If the unlock operation fails.

## function `tryLockExclusive`

Attempt to acquire an exclusive lock without blocking. Returns immediately whether or not the lock was acquired.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to attempt to lock.`

**Returns**: — True if the exclusive lock was successfully acquired, False if the
lock is held by another process.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function tryLockExclusive( I64 fd ) -> Boolean`

Attempt to acquire an exclusive lock without blocking. Returns immediately whether or not the lock was acquired.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor to attempt to lock.`

**Returns**: — True if the exclusive lock was successfully acquired, False if the
lock is held by another process.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Pipe`

A unidirectional byte pipe backed by two file descriptors (one for reading, one for writing). Created with the O_CLOEXEC flag so both ends are automatically closed on exec. Useful for inter-process or inter-thread communication.

### Fields

| Name | Type | Access |
|------|------|--------|
| `readFd` | `I64` | protect |
| `writeFd` | `I64` | protect |
| `closed` | `Boolean` | protect |

### Methods

#### `function Pipe( self ) -> Void`

Create a new pipe with the O_CLOEXEC flag set on both ends. Allocates temporary memory for the pipe2 syscall, reads the two file descriptors, and frees the temporary buffer.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getReadFd( self ) -> I64`

Return the file descriptor for the read end of the pipe.

**Returns**: — The read-end file descriptor.

#### `function getWriteFd( self ) -> I64`

Return the file descriptor for the write end of the pipe.

**Returns**: — The write-end file descriptor.

#### `function readBytes( self, Memory<I64> buf, I64 length ) -> I64`

Read up to the specified number of bytes from the read end of the pipe into a Memory buffer. Uses a temporary buffer for the raw read, then copies bytes element-by-element into the destination.

**Parameters**:

- `buf` (`Memory<I64>`)
- `The destination buffer to read bytes into.`
- `length` (`I64`)
- `The maximum number of bytes to read.`

**Returns**: — The actual number of bytes read, which may be less than length.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function writeBytes( self, Memory<I64> buf, I64 length ) -> I64`

Write the specified number of bytes from a Memory buffer to the write end of the pipe. Copies bytes into a temporary buffer before writing.

**Parameters**:

- `buf` (`Memory<I64>`)
- `The source buffer containing bytes to write.`
- `length` (`I64`)
- `The number of bytes to write.`

**Returns**: — The actual number of bytes written to the pipe.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function writeString( self, String data ) -> I64`

Write all bytes of a string to the write end of the pipe.

**Parameters**:

- `data` (`String`)
- `The string to write to the pipe.`

**Returns**: — The number of bytes written.

#### `function closeRead( self ) -> Void`

Close only the read end of the pipe. Useful when a process only needs the write end after forking.

#### `function closeWrite( self ) -> Void`

Close only the write end of the pipe. Useful when a process only needs the read end after forking.

#### `function close( self ) -> Void`

Close both ends of the pipe. Subsequent calls after the first close are no-ops.

#### `function destroy( self ) -> Void`

Destructor that delegates to close to ensure resource cleanup.

## function `getTerminalSize`

Query the terminal dimensions using the TIOCGWINSZ ioctl on stdout. Returns both rows and columns packed into a single I64 value: the lower 32 bits contain the row count and the upper 32 bits contain the column count.

**Returns**: — A packed I64 where (result & 4294967295) gives rows and
(result / 4294967296) gives columns.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getTerminalSize(  ) -> I64`

Query the terminal dimensions using the TIOCGWINSZ ioctl on stdout. Returns both rows and columns packed into a single I64 value: the lower 32 bits contain the row count and the upper 32 bits contain the column count.

**Returns**: — A packed I64 where (result & 4294967295) gives rows and
(result / 4294967296) gives columns.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `getTerminalRows`

Return the number of rows in the terminal.

**Returns**: — The terminal row count.

### Methods

#### `function getTerminalRows(  ) -> I64`

Return the number of rows in the terminal.

**Returns**: — The terminal row count.

## function `getTerminalCols`

Return the number of columns in the terminal.

**Returns**: — The terminal column count.

### Methods

#### `function getTerminalCols(  ) -> I64`

Return the number of columns in the terminal.

**Returns**: — The terminal column count.

## function `setRawMode`

Set the terminal associated with the given file descriptor to raw mode by disabling the ECHO and ICANON flags in the termios local flags. Saves and returns the original termios settings so they can be restored later with restoreMode.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor of the terminal to configure.`

**Returns**: — A Memory buffer containing the saved original termios settings.
Pass this to restoreMode to restore the terminal to its previous state.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function setRawMode( I64 fd ) -> Memory<I64>`

Set the terminal associated with the given file descriptor to raw mode by disabling the ECHO and ICANON flags in the termios local flags. Saves and returns the original termios settings so they can be restored later with restoreMode.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor of the terminal to configure.`

**Returns**: — A Memory buffer containing the saved original termios settings.
Pass this to restoreMode to restore the terminal to its previous state.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `restoreMode`

Restore terminal settings from a previously saved termios buffer, undoing the effect of setRawMode. Frees the saved buffer after restoring.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor of the terminal to restore.`
- `saved` (`Memory<I64>`)
- `The saved termios buffer returned by setRawMode. This buffer`
- `is freed after use and must not be used again.`

### Methods

#### `function restoreMode( I64 fd, Memory<I64> saved ) -> Void`

Restore terminal settings from a previously saved termios buffer, undoing the effect of setRawMode. Frees the saved buffer after restoring.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor of the terminal to restore.`
- `saved` (`Memory<I64>`)
- `The saved termios buffer returned by setRawMode. This buffer`
- `is freed after use and must not be used again.`

## function `chmod`

Change the permission mode of a file or directory.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file or directory.`
- `mode` (`I64`)
- `The new permission mode bits` (`e.g., 493 for 0755`)

**Raises**:

- `IOError` → `Error` — If the chmod operation fails.

### Methods

#### `function chmod( String path, I64 mode ) -> Void`

Change the permission mode of a file or directory.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file or directory.`
- `mode` (`I64`)
- `The new permission mode bits` (`e.g., 493 for 0755`)

**Raises**:

- `IOError` → `Error` — If the chmod operation fails.

## function `fchmod`

Change the permission mode of a file referenced by file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor of the open file.`
- `mode` (`I64`)
- `The new permission mode bits.`

**Raises**:

- `IOError` → `Error` — If the fchmod operation fails.

### Methods

#### `function fchmod( I64 fd, I64 mode ) -> Void`

Change the permission mode of a file referenced by file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor of the open file.`
- `mode` (`I64`)
- `The new permission mode bits.`

**Raises**:

- `IOError` → `Error` — If the fchmod operation fails.

## function `chown`

Change the owner and group of a file or directory.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file or directory.`
- `uid` (`I64`)
- `The new owner user ID.`
- `gid` (`I64`)
- `The new group ID.`

**Raises**:

- `IOError` → `Error` — If the chown operation fails.

### Methods

#### `function chown( String path, I64 uid, I64 gid ) -> Void`

Change the owner and group of a file or directory.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file or directory.`
- `uid` (`I64`)
- `The new owner user ID.`
- `gid` (`I64`)
- `The new group ID.`

**Raises**:

- `IOError` → `Error` — If the chown operation fails.

## function `fchown`

Change the owner and group of a file referenced by file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor of the open file.`
- `uid` (`I64`)
- `The new owner user ID.`
- `gid` (`I64`)
- `The new group ID.`

**Raises**:

- `IOError` → `Error` — If the fchown operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function fchown( I64 fd, I64 uid, I64 gid ) -> Void`

Change the owner and group of a file referenced by file descriptor.

**Parameters**:

- `fd` (`I64`)
- `The file descriptor of the open file.`
- `uid` (`I64`)
- `The new owner user ID.`
- `gid` (`I64`)
- `The new group ID.`

**Raises**:

- `IOError` → `Error` — If the fchown operation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `FilesystemInfo`

Immutable snapshot of filesystem statistics as returned by the statfs syscall. Contains information about the filesystem type, block sizes, and space/inode availability.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fsType` | `I64` | public |
| `blockSize` | `I64` | public |
| `totalBlocks` | `I64` | public |
| `freeBlocks` | `I64` | public |
| `availableBlocks` | `I64` | public |
| `totalFiles` | `I64` | public |
| `freeFiles` | `I64` | public |

### Methods

#### `function FilesystemInfo( self, I64 fsType, I64 blockSize, I64 totalBlocks, I64 freeBlocks, I64 availableBlocks, I64 totalFiles, I64 freeFiles ) -> Void`

Construct a new FilesystemInfo with all filesystem statistics.

**Parameters**:

- `fsType` (`I64`)
- `The filesystem type identifier.`
- `blockSize` (`I64`)
- `The optimal transfer block size in bytes.`
- `totalBlocks` (`I64`)
- `The total number of data blocks.`
- `freeBlocks` (`I64`)
- `The number of free blocks for all users.`
- `availableBlocks` (`I64`)
- `The number of free blocks for unprivileged users.`
- `totalFiles` (`I64`)
- `The total number of inodes.`
- `freeFiles` (`I64`)
- `The number of free inodes.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getFsType( self ) -> I64`

Return the filesystem type identifier.

**Returns**: — The filesystem type magic number.

#### `function getBlockSize( self ) -> I64`

Return the optimal transfer block size in bytes.

**Returns**: — The block size in bytes.

#### `function getTotalBlocks( self ) -> I64`

Return the total number of data blocks on the filesystem.

**Returns**: — The total block count.

#### `function getFreeBlocks( self ) -> I64`

Return the number of free blocks available to all users.

**Returns**: — The free block count.

#### `function getAvailableBlocks( self ) -> I64`

Return the number of free blocks available to unprivileged users.

**Returns**: — The available block count (may be less than free blocks due
to reserved space).

#### `function getTotalFiles( self ) -> I64`

Return the total number of inodes on the filesystem.

**Returns**: — The total inode count.

#### `function getFreeFiles( self ) -> I64`

Return the number of free inodes available.

**Returns**: — The free inode count.

#### `function getTotalSpace( self ) -> I64`

Calculate the total storage space on the filesystem in bytes.

**Returns**: — The total space in bytes (totalBlocks * blockSize).

#### `function getFreeSpace( self ) -> I64`

Calculate the free storage space on the filesystem in bytes.

**Returns**: — The free space in bytes (freeBlocks * blockSize).

#### `function getAvailableSpace( self ) -> I64`

Calculate the storage space available to unprivileged users in bytes.

**Returns**: — The available space in bytes (availableBlocks * blockSize).

## function `statfs`

Retrieve filesystem statistics for the filesystem containing the given path. Parses the Linux statfs struct (x86_64 layout) into a FilesystemInfo object.

**Parameters**:

- `path` (`String`)
- `Any path on the target filesystem.`

**Returns**: — A FilesystemInfo object with the filesystem statistics.

**Raises**:

- `IOError` → `Error` — If the statfs syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function statfs( String path ) -> FilesystemInfo`

Retrieve filesystem statistics for the filesystem containing the given path. Parses the Linux statfs struct (x86_64 layout) into a FilesystemInfo object.

**Parameters**:

- `path` (`String`)
- `Any path on the target filesystem.`

**Returns**: — A FilesystemInfo object with the filesystem statistics.

**Raises**:

- `IOError` → `Error` — If the statfs syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `getcwd`

Return the current working directory as a String.

**Returns**: — The absolute path of the current working directory.

**Raises**:

- `IOError` → `Error` — If the getcwd syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function getcwd(  ) -> String`

Return the current working directory as a String.

**Returns**: — The absolute path of the current working directory.

**Raises**:

- `IOError` → `Error` — If the getcwd syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `chdir`

Change the current working directory to the specified path.

**Parameters**:

- `path` (`String`)
- `The filesystem path to change to.`

**Raises**:

- `IOError` → `Error` — If the chdir syscall fails.

### Methods

#### `function chdir( String path ) -> Void`

Change the current working directory to the specified path.

**Parameters**:

- `path` (`String`)
- `The filesystem path to change to.`

**Raises**:

- `IOError` → `Error` — If the chdir syscall fails.

## function `tempFile`

Create a temporary file in /tmp with a unique name constructed from the given prefix and a numeric suffix derived from the process ID. Tries up to 1000 candidate names using O_CREAT | O_EXCL to avoid race conditions.

**Parameters**:

- `prefix` (`String`)
- `A prefix string for the temporary filename` (`e.g., "myapp_"`)

**Returns**: — A File handle opened in read-write mode for the newly created
temporary file.

**Raises**:

- `IOError` → `Error` — If a unique temporary file cannot be created after 1000 attempts.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function tempFile( String prefix ) -> File`

Create a temporary file in /tmp with a unique name constructed from the given prefix and a numeric suffix derived from the process ID. Tries up to 1000 candidate names using O_CREAT | O_EXCL to avoid race conditions.

**Parameters**:

- `prefix` (`String`)
- `A prefix string for the temporary filename` (`e.g., "myapp_"`)

**Returns**: — A File handle opened in read-write mode for the newly created
temporary file.

**Raises**:

- `IOError` → `Error` — If a unique temporary file cannot be created after 1000 attempts.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `tempDir`

Create a temporary directory in /tmp with a unique name constructed from the given prefix and a numeric suffix derived from the process ID. Tries up to 1000 candidate names. The directory is created with permission mode 0700 (owner-only access).

**Parameters**:

- `prefix` (`String`)
- `A prefix string for the temporary directory name` (`e.g., "myapp_"`)

**Returns**: — The filesystem path of the newly created temporary directory.

**Raises**:

- `IOError` → `Error` — If a unique temporary directory cannot be created after 1000 attempts.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function tempDir( String prefix ) -> String`

Create a temporary directory in /tmp with a unique name constructed from the given prefix and a numeric suffix derived from the process ID. Tries up to 1000 candidate names. The directory is created with permission mode 0700 (owner-only access).

**Parameters**:

- `prefix` (`String`)
- `A prefix string for the temporary directory name` (`e.g., "myapp_"`)

**Returns**: — The filesystem path of the newly created temporary directory.

**Raises**:

- `IOError` → `Error` — If a unique temporary directory cannot be created after 1000 attempts.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `listDirectory`

List all entries in a directory, excluding "." and "..".

Uses the getdents64 syscall to read directory entries directly. Each entry name is extracted from the raw dirent64 struct.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to list.`

**Returns**: `ArrayList<String>` — A list of entry names (files, subdirectories, etc.).

**Raises**:

- `IOError` → `Error` — If the directory cannot be opened or read.

**Complexity**:
- Time: `O(n) where n is the number of directory entries`
- Space: `O(n)`

### Methods

#### `function listDirectory( String path ) -> ArrayList<String>`

List all entries in a directory, excluding "." and "..".

Uses the getdents64 syscall to read directory entries directly. Each entry name is extracted from the raw dirent64 struct.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to list.`

**Returns**: `ArrayList<String>` — A list of entry names (files, subdirectories, etc.).

**Raises**:

- `IOError` → `Error` — If the directory cannot be opened or read.

**Complexity**:
- Time: `O(n) where n is the number of directory entries`
- Space: `O(n)`

## function `makeDirectories`

Create a directory and all necessary parent directories.

Walks the path from left to right, creating each component that does not yet exist. Silently succeeds if the directory already exists.

**Parameters**:

- `path` (`String`)
- `The directory path to create, including all parents.`
- `mode` (`I64`)
- `The permission mode bits for newly created directories`

**Raises**:

- `IOError` → `Error` — If a directory component cannot be created.

**Complexity**:
- Time: `O(d) where d is the depth of the path`

### Methods

#### `function makeDirectories( String path, I64 mode ) -> Void`

Create a directory and all necessary parent directories.

Walks the path from left to right, creating each component that does not yet exist. Silently succeeds if the directory already exists.

**Parameters**:

- `path` (`String`)
- `The directory path to create, including all parents.`
- `mode` (`I64`)
- `The permission mode bits for newly created directories`

**Raises**:

- `IOError` → `Error` — If a directory component cannot be created.

**Complexity**:
- Time: `O(d) where d is the depth of the path`

